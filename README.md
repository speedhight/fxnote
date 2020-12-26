# fxnote
typing board
//Standard javafx imports.
import javafx.application.Application;
import javafx.stage.Stage;
import javafx.scene.Scene;


//Imports for components in this application.
import javafx.scene.control.Label;
import javafx.scene.control.Button;
import javafx.scene.control.TextArea;
import javafx.scene.control.Menu;
import javafx.scene.control.MenuItem;
import javafx.scene.control.MenuBar;
import javafx.application.Platform;

//Dialogs...
import javafx.scene.control.Alert;
import javafx.scene.control.Alert.AlertType;



//Imports for file support.
import javafx.stage.FileChooser;
import java.io.File;
import java.io.FileReader;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.FileOutputStream;





//Imports for layout.
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.HBox;
import javafx.scene.layout.VBox;

import javafx.geometry.Insets;




public class FXNotes2 extends Application {
	
	//Declare components at class scope.
	TextArea txtMain;
	MenuBar mBar;
	
	//Name for the file being worked on.
	//At startup, this name is empty.
	File savedFile;
	
	
	
	
	
	


	public FXNotes2() {
		// TODO Auto-generated constructor stub
	}//constructor
	
	@Override
	public void init(){
		
		//Instantiate components with the word 'new'.
		txtMain = new TextArea();
		
		mBar = new MenuBar();
		
		//Now create menus.
		//The file menu.
		Menu fileMnu = new Menu("File");
		
		//Add menuitems to the file menu.
		MenuItem newItem = new MenuItem("New");
		fileMnu.getItems().add(newItem);
		
		MenuItem openItem = new MenuItem("Open");
		fileMnu.getItems().add(openItem);
		openItem.setOnAction(ae -> doFileOpen());
		
		MenuItem saveItem = new MenuItem("Save");
		fileMnu.getItems().add(saveItem);
		saveItem.setOnAction(ae -> saveFile(savedFile));
		
		MenuItem saveAsItem = new MenuItem("Save As...");
		fileMnu.getItems().add(saveAsItem);
		saveAsItem.setOnAction(ae -> doSaveAs(savedFile));
		
		
		MenuItem closeItem = new MenuItem("Close");
		fileMnu.getItems().add(closeItem);
		closeItem.setOnAction(ae -> txtMain.clear());
				
		MenuItem exitItem = new MenuItem("Exit");
		fileMnu.getItems().add(exitItem);
		exitItem.setOnAction(ae -> Platform.exit());
		
		
		
		Menu editMnu = new Menu("Edit");
		MenuItem undoItem = new MenuItem("Undo");
		editMnu.getItems().add(undoItem);
		undoItem.setOnAction(ae -> txtMain.undo());
		
		
		
		MenuItem redoItem = new MenuItem("Redo");
		editMnu.getItems().add(redoItem);
		redoItem.setOnAction(abc -> txtMain.redo());
		
		MenuItem cutItem = new MenuItem("Cut");
		editMnu.getItems().add(cutItem);
		cutItem.setOnAction(ae -> txtMain.cut());
		
		MenuItem copyItem = new MenuItem("Copy");
		editMnu.getItems().add(copyItem);
		copyItem.setOnAction(ae -> txtMain.copy());
		
		MenuItem pasteItem = new MenuItem("Paste");
		editMnu.getItems().add(pasteItem);
		pasteItem.setOnAction(ae -> txtMain.paste());
				
		MenuItem deleteItem = new MenuItem("Delete");
		editMnu.getItems().add(deleteItem);
		deleteItem.setOnAction(ae -> txtMain.deleteText(txtMain.getSelection()));
		
		MenuItem selectAllItem = new MenuItem("Select All");
		editMnu.getItems().add(selectAllItem);
		selectAllItem.setOnAction(ae -> txtMain.selectAll());
				
		
		Menu optMnu = new Menu("Options");
		
		Menu hlpMnu = new Menu("Help");
		
		MenuItem aboutItem = new MenuItem("About FXNotes");
		hlpMnu.getItems().add(aboutItem);
		aboutItem.setOnAction(ae -> showAlert());
			
		
		
		
		//Add all menus.
		mBar.getMenus().add(fileMnu);
		mBar.getMenus().add(editMnu);
		mBar.getMenus().add(optMnu);
		mBar.getMenus().add(hlpMnu);
				
		
		
	}//init()
	
	public void saveFile(File f) {
		
		f = savedFile;
				
		//File already assigned but check just in case.
		if(savedFile != null) {
			
			//Appear to have a valid file. Try to save the file.
			try {
				
				FileOutputStream fos = new FileOutputStream(savedFile, false);
				String text = txtMain.getText();
				byte[] dataOut = text.getBytes();
				fos.write(dataOut);
				fos.flush();
				fos.close();
				
				
			}//try
			catch(IOException ioe) {
				System.err.println("Error writing file:");
				ioe.getMessage();
								
			}//catch()
			
			
		}
		else {
			Alert alert = new Alert(AlertType.ERROR);
			alert.setTitle("Assign a file name");
			alert.setContentText("Assign a file name. Click on 'Save As..'");
			
			alert.show();
			
		}
		
		
	}//saveFile()
	
	public void doSaveAs(File f) {
		//f is the filename.
		
		//A filechooser to choose the path for the file.
		FileChooser fc = new FileChooser();
		
		savedFile = fc.showSaveDialog(null);
		
		if(savedFile != null) {
			
			//Try to save the file.
			try {
				FileOutputStream fos = new FileOutputStream(savedFile, true);
				String text = txtMain.getText();
				byte[] dataOut = text.getBytes();
				fos.write(dataOut);
				fos.flush();
				fos.close();						
				
			}//try
			catch(IOException ioe) {
				System.out.println("Error writing file:" + ioe.getMessage());
				
			}//catch
			
		}//if
		
	}//doSaveAs()
	
	
	
	public void showAlert() {
		Alert alert = new Alert(AlertType.INFORMATION);
		alert.setTitle("About FXNotes");
		alert.setContentText("FXNotes V1.0.0\n GCD 2019");
				
		alert.show();
				
	}//showAlert()
	
	public void showAboutDialog(){
		
		//Create a stage.
		Stage dialogStage = new Stage();
		dialogStage.setWidth(300);
		dialogStage.setHeight(200);
		dialogStage.setTitle("About FXNotes");
		dialogStage.setResizable(false);
		
		//Label for a message.
		Label lblAbout = new Label("FXNotes 2019");
		
		//OK button.
		Button btnOK = new Button("OK");
		
		//Handle events on the button.
		btnOK.setOnAction(ae -> dialogStage.close());
		
		//Layout for the dialog.
		BorderPane bpDialog = new BorderPane();
		bpDialog.setCenter(lblAbout);
		
		BorderPane bpButton = new BorderPane();
		bpButton.setCenter(btnOK);
		
		//VBox as main container.
		VBox vb = new VBox();
		
		//Add components to the vbox.
		vb.getChildren().addAll(bpDialog,bpButton);
		
		//Create a scene.
		Scene s = new Scene(vb);
		
		//Set the scene.
		dialogStage.setScene(s);
		
		//Show the stage.
		dialogStage.show();
			
		
		
	}//showAboutDialog()
	
	public void doFileOpen(){
		
		//Create a filechooser dialog.
		FileChooser fChooser = new FileChooser();
		
		//Create a file instance. Assign the file from the fc.
		savedFile = fChooser.showOpenDialog(null);
		
		//Check if the file is correctly chosen.
		if(savedFile != null){
			
			//Try to open the file and show it in the text area.
			try{
				StringBuilder stringBldr = new StringBuilder();
				FileReader fReader = new FileReader(savedFile);
				BufferedReader bReader = new BufferedReader(fReader);
				
				//A string to store lines of text.
				String line;
				
				//Iterate through the file reading one line at a time.
				while((line=bReader.readLine()) != null){
					//Append a newline character.
					line = line + "\n";
					
					//Collect text in the string builder.
					stringBldr.append(line);										
					
				}//while(reading file)
				
				//Got all of the text. Now show it in txtMain.
				txtMain.setText(stringBldr.toString());
				
				
				//Important: Close the file.
				bReader.close();
				
			}//try
			catch(IOException ioe){
				System.err.println("Error reading file:");
				ioe.printStackTrace();
				
			}//catch
			
		}//if fileToOpen
		
		
		
		
		
		
	}//doFileOpen()
	
	
	


	@Override
	public void start(Stage pStage) throws Exception {
		//Set the title.
		pStage.setTitle("FXNotes V2.0.0.0");
		
		//Set the width and height.
		pStage.setWidth(400);
		pStage.setHeight(300);
		
		//Create a layout.
		BorderPane bp = new BorderPane();
		
		
		//Add components to the layout.
		bp.setCenter(txtMain);
		bp.setTop(mBar);
		
		
		
		//Create a scene.
		Scene s = new Scene(bp);
		
		//Set the scene.
		pStage.setScene(s);
		
		//Show the stage.
		pStage.show();

	}//start()
	
	@Override
	public void stop(){
		
	}//stop()

	public static void main(String[] args) {
		//Launch the application.
		launch();
		

	}//main()

}//class
