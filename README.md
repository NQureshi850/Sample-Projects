# Sample-Projects
These are some of my projects that I have completed during my Computer Programming 3 class

/*Nameer Qureshi
 * 5/8/15
 * Period 5
 * This class represents a DJ Store that will allow the user to sell and add records at their leisure
 */

import java.awt.*;
import java.awt.event.*;
import java.awt.image.*;
import java.io.*;
import java.text.*;
import java.util.*;

import javax.imageio.*;
import javax.swing.*;
import javax.swing.table.*;

public class Qureshi_UberMegaDJStore extends JFrame implements ActionListener{

	private String fileName;
	private File currentFile;
	private JTable table;
	private DefaultTableModel tableModel;
	private DefaultComboBoxModel comboBoxModel;
	private ArrayList<JTextField> addRecordTextFields;
	private JComboBox sellRecordComboBox;
	private JTextField toBuy;
	private JLabel available;
	private JLabel price;

	//initializes the instance variables and creatse the GUI
	public Qureshi_UberMegaDJStore()
	{
		fileName = "Untitled*";
		currentFile = null;
		
		setTitle("DJ Emporium ---" + fileName);
		setSize(600, 450);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		
		PicPanel frameBackground = new PicPanel("frameBackground.jpg");
		frameBackground.setLayout(null);
		
		JLabel title = new JLabel("The DJ Emporium");
		title.setBounds(200, 40, 200, 50);
		title.setFont(new Font(null, Font.BOLD, 20));
		title.setForeground(Color.MAGENTA);
		
		add(title);
		
		JMenuBar options = new JMenuBar();
		JMenu file = new JMenu("File");
		
		String[] fileOptions = {"Open", "Save", "Close", "Exit"};
		
		for(int i = 0; i < 4; i++)
		{
			JMenuItem item = new JMenuItem(fileOptions[i]);
			item.addActionListener(this);
			file.add(item);
		}
		
		options.add(file);
		setJMenuBar(options);
		
		JTabbedPane tabs = new JTabbedPane();
		tabs.setBounds(35, 130, 520, 230);
		
		//frameBackground.add(tabs);
		
		//------------------------------creates the "Inventory panel"-------------------------------------
		PicPanel inventoryPanel = new PicPanel("panel1Background.jpg");
		inventoryPanel.setLayout(null);
		String[] columns = {"SKU", "Artist", "Album", "Price", "Quantity"};
		
		tableModel = new DefaultTableModel(columns, 0)
		{
			public boolean isCellEditable(int row, int column){
				return false;
			}
			
		};
		
		
		table = new JTable(tableModel);
		table.getTableHeader().setReorderingAllowed(false);
		
		JScrollPane scroller = new JScrollPane(table);
		scroller.setBounds(20,20,450,150);
		inventoryPanel.add(scroller);
		
		
		//----------------------------creates the "Add A Record" panel---------------------------------------------------------
		PicPanel addRecordPanel = new PicPanel("panel2Background.jpg");
		addRecordPanel.setLayout(null);
		
		//adds the textfields in the order: Sku, Artist, Album, Price, Quantity
		addRecordTextFields = new ArrayList<JTextField>();
		int verticalAlignment = 20;
		int horizAlignment = 10;

		//creates the labels for the leftside with textfields
		for(int i = 0; i < 3; i++)
		{
			//adds the leftside labels with text Fields
			
			JLabel leftLabel = new JLabel(columns[i]);
			leftLabel.setBounds(horizAlignment, verticalAlignment, 60, 20);
			leftLabel.setForeground(Color.MAGENTA);
			addRecordPanel.add(leftLabel);
				
			JTextField fieldToAdd = new JTextField();
			fieldToAdd.setBounds(horizAlignment + 50, verticalAlignment, 60, 20);
			fieldToAdd.addActionListener(this);
			addRecordTextFields.add(fieldToAdd);
			addRecordPanel.add(fieldToAdd);
			
			verticalAlignment+=50;
		}
		
		horizAlignment = 240;
		
		//creates the labels for the rightside with textfields
		for(int i = 3; i < 5; i++)
		{

			JLabel rightLabel = new JLabel(columns[i]);
			rightLabel.setBounds(horizAlignment, verticalAlignment-150, 60, 20);
			rightLabel.setForeground(Color.MAGENTA);
			addRecordPanel.add(rightLabel);
				
			JTextField fieldToAdd = new JTextField();
			fieldToAdd.setBounds(horizAlignment + 50, verticalAlignment-150, 60, 20);
			fieldToAdd.addActionListener(this);
			addRecordTextFields.add(fieldToAdd);
			addRecordPanel.add(fieldToAdd);
			
			verticalAlignment+=50;
		}
		
		JButton addButton = new JButton("Add");
		addButton.setBounds(240, 125, 80, 25);
		addButton.addActionListener(this);
		
		addRecordPanel.add(addButton);
		
		//-----------------------------creates the "Sell A Record" panel-------------------------------
		PicPanel sellRecordPanel = new PicPanel("panel3Background.jpg");
		sellRecordPanel.setLayout(null);
		
		String[] albums = new String[table.getRowCount()];
		
		//retrieves the artist and album information from the table
		for(int i = 0; i < table.getRowCount(); i++)
			albums[i] = ((String)table.getValueAt(i, 1)) + ((String)table.getValueAt(i, 2));
		
		
		comboBoxModel = new DefaultComboBoxModel(albums);
		sellRecordComboBox = new JComboBox(comboBoxModel);
		
		sellRecordComboBox.addActionListener(this);
		sellRecordComboBox.setBounds(20, 10, 200, 20);
		sellRecordComboBox.setActionCommand("sellRecordComboBox");
		sellRecordPanel.add(sellRecordComboBox);
		
		//creates the labels for the sellRecordPanel ******edit******
		available = new JLabel("Available   ");
		available.setBounds(290, 20, 100, 20);
		available.setForeground(Color.MAGENTA);
		price = new JLabel("Price     ");
		price.setBounds(290, 40, 100, 20);
		price.setForeground(Color.MAGENTA);
		JLabel quantity = new JLabel("Quantity");
		quantity.setBounds(290, 110, 50, 20);
		quantity.setForeground(Color.MAGENTA);
		
		toBuy = new JTextField();
		toBuy.setBounds(340, 110, 70, 23);
		toBuy.addActionListener(this);
		
		JButton sellButton = new JButton("Sell");
		sellButton.setBounds(310, 150, 80, 25);
		sellButton.addActionListener(this);
	
		sellRecordPanel.add(sellButton);
		sellRecordPanel.add(available);
		sellRecordPanel.add(price);
		sellRecordPanel.add(quantity);
		sellRecordPanel.add(toBuy);
		
		//----------------------------------------------------------------------------------------------
		
		tabs.add("Inventory", inventoryPanel);
		tabs.add("Add A Record", addRecordPanel);
		tabs.add("Sell A Record", sellRecordPanel);
		
		frameBackground.add(tabs);
		
		add(frameBackground);
		
		setVisible(true);
	}
	
	//draws the images for each individual panel
	class PicPanel extends JPanel{

		private BufferedImage image;
		private int w,h;
		
		public PicPanel(String fname){

			//reads the image
			try {
				image = ImageIO.read(new File(fname));
				w = image.getWidth();
				h = image.getHeight();

			} catch (IOException ioe) {
				System.out.println("Could not read in the pic");
				System.exit(0);
			}

		}

		public Dimension getPreferredSize() {
			return new Dimension(w,h);
		}
		
		//this will draw the image
		public void paintComponent(Graphics g){
			super.paintComponent(g);
			g.drawImage(image,0,0,this);
		}
	}
	
		//finds the location of the artist to find
		private int findSku(String album)
		{
			for(int i = 0; i < table.getRowCount(); i++)
			{
				String toExamine = (String)table.getValueAt(i, 2);
				
				if(toExamine.equals(album))
					return i;
			}
			
			return -1;
		}
		
		//prints out the message stating a number is negative
		private void errorMessageNegative()
		{
			JOptionPane.showMessageDialog(null, "The price or quantity is negative");
		}
		
		//prints out an error message stating one of the textFields is empty
		private void errorMessageEmpty()
		{
			JOptionPane.showMessageDialog(null, "One of the text fields is empty, you tomato");
		}
		
		//prints out a message indicating a successful operation
		private void messageOperationSuccess()
		{
			JOptionPane.showMessageDialog(null, "The operation was a success, doctor");
		}
		
		//prints out an error message indicating they put letters in the fields requiring numbers
		private void errorMessageIncorrectFormat()
		{
			JOptionPane.showMessageDialog(null, "Price and Quantity fields require numbers, not letters");
		}
		
		//returns true if any of the text fields are empty
		private boolean checkEmptyFieldsAddRecord()
		{
			for(int i = 0; i < addRecordTextFields.size(); i++)
			{
				String toExamine = addRecordTextFields.get(i).getText();
				
				if(toExamine.equals("") || toExamine.equals(null)) //|| toExamine.equals)
					return true;
			}
			
			return false;
		}
		
		//returns true if the toBuy field is empty
		private boolean checkEmptyFieldSellRecord()
		{
			if(toBuy.equals("") || toBuy.equals(null))
				return true;
			
			else
				return false;
		}
		
		//returns true if the number is negative, false otherwise
		private boolean checkNeg(int num)
		{
			return num < 0;
		}
		
		//returns true if the number is negative, false otherwise (overloaded)
		private boolean checkNeg(double num)
		{
			return num < 0;
		}
		
		//returns true if they incorrectly inputted values in the SKU, quantity, or price field for AddRecord
		private boolean checkNumber(String toCheck)
		{
			try
			{
				Double.parseDouble(toCheck);
			}catch(NumberFormatException e)
			{
				return true;
			}
			
			return false;
		}
		
		//clears the information out from the textFields
		private void clearTextFields()
		{
			for(int i = 0; i < addRecordTextFields.size(); i++)
			{
				JTextField fieldToClear = addRecordTextFields.get(i);
				fieldToClear.setText("");
			}
		}
		
		//sets the text of the labels of the sellRecord tab to represent the correct amount
		private void setLabels(String quantityToSet, String priceToSet)
		{
			available.setText("Available:     " + quantityToSet);
			price.setText("Price:      " + priceToSet);
		}
		
		//adds an asterisk to the top right corner
		private void fileChanged()
		{
			if(fileName.indexOf("*") == -1)
				fileName += "*";
			
		}
		
			
		//grabs the associated fileand returns it
		private void grabFile(int r, JFileChooser fc)
		{
			if(r == JFileChooser.APPROVE_OPTION)
				currentFile = fc.getSelectedFile();
	
		}
	
		//saves the save appropriately
		private void saveFile(File fileToSave)
		{
			if(fileName.equals("Untitled*"))
			{
				JFileChooser fileChooser = new JFileChooser();
				int result = fileChooser.showSaveDialog(null);
				
				grabFile(result, fileChooser);

			}
			
			try
			{
				//look back on text editor
				FileWriter fw = new FileWriter(currentFile);
				
				for(int row = 0; row < tableModel.getRowCount(); row++)
				{
					for(int col = 0; col < tableModel.getColumnCount(); col++)
					{
						fw.write(String.valueOf(tableModel.getValueAt(row, col) + "\r\n"));
					}
					
				
				}
				
				fw.close();
				
			}catch(IOException e){
				
				System.out.println("IO Issue");
				System.exit(-1);
			}
			
			removeAsterisk();
		}
		
		//removes the asterisk from the file indicating a save has been made
		private void removeAsterisk()
		{
			if(fileIsChanged() == true)
			{
				fileName = fileName.substring(0, fileName.length()-1);
			}
		}
		
		//returns true if the file has not been saved
		private boolean fileIsChanged()
		{
			if(fileName.indexOf("*") != -1)
				return true;
			
			
			else
				return false;
		}
		
		//prompts the user to save if their file has been changed
		private void promptSave()
		{
			if(fileIsChanged() == true)
			{
				int result = JOptionPane.showConfirmDialog(null, "Do you want to save your current work?");
				
				if(result == JOptionPane.YES_OPTION)
				{
					saveFile(currentFile);
				}
				
			}
		}
		
		//clears the data from the table and comboBox
		private void clearData()
		{
			int numRows = tableModel.getRowCount();
			
			for(int i = 0; i < numRows; i++)
			{
		    	tableModel.removeRow(i);
		    	i--;
			}
			
			comboBoxModel.removeAllElements();
		}
		
		//will perform the indicated action based on user seleccted and input
		public void actionPerformed(ActionEvent ae)
		{
			DecimalFormat formatter = new DecimalFormat("#.00");
			
			if(ae.getActionCommand().equals("Add"))
			{
				String priceField = addRecordTextFields.get(3).getText();
				String quantityField = addRecordTextFields.get(4).getText();
				String skuField = addRecordTextFields.get(0).getText();
				
				
				//firsts checks if any field is empty before proceeding to to anything else
				if(checkEmptyFieldsAddRecord() == true)
					errorMessageEmpty();
				
				else if(checkNumber(priceField) == true || checkNumber(quantityField) == true || checkNumber(skuField) == true)
					errorMessageIncorrectFormat();
				
				else
				{
					double price = Double.parseDouble(priceField);
					int quantity = Integer.parseInt(quantityField);
					String album = addRecordTextFields.get(2).getText();
					int sku = findSku(album);
					
					if(checkNeg(price) == true || checkNeg(quantity) == true)
						errorMessageNegative();
						
					//if album already exists
					else if(sku != -1)
					{
						int currentQuantity =  Integer.parseInt((String) table.getValueAt(sku, 4));
						int newQuantity = currentQuantity + quantity;
						
						table.setValueAt(newQuantity, sku, 4);
						
						clearTextFields();
						messageOperationSuccess();
						fileChanged();
					}
						
					//if album does not already exist
					else
					{
						String artistToAdd = addRecordTextFields.get(1).getText();
						
						Object[] rowToAdd = {skuField, artistToAdd, album, formatter.format(price), quantity};
						tableModel.addRow(rowToAdd);
						
						Object elementToAdd = artistToAdd + " --- " + album;
						comboBoxModel.addElement(elementToAdd);
						
						clearTextFields();
						messageOperationSuccess();
						fileChanged();
					}
				}
			}		
			
			//updates the information based on what record is selected
			else if(ae.getActionCommand().equals("sellRecordComboBox"))
			{
				int index = sellRecordComboBox.getSelectedIndex();
				String availableQuantity =  Integer.toString((Integer) table.getValueAt(index, 4)); 
				String priceOfAlbum = (String) table.getValueAt(index, 3);
				
				setLabels(availableQuantity, priceOfAlbum);
			}
			
			//handles the "Sell" operation
			else if(ae.getActionCommand().equals("Sell"))
			{
				
				if(checkEmptyFieldSellRecord() == true)
					errorMessageEmpty();
				
				else
				{
					int quantityToBuy = Integer.parseInt(toBuy.getText());
					int index = sellRecordComboBox.getSelectedIndex();
					int availableQuantity = Integer.parseInt(available.getText().substring(15)); 
					double priceOfAlbum = Double.parseDouble(price.getText().substring(12));
					
					setLabels(Integer.toString(availableQuantity), Double.toString(priceOfAlbum));
					
					if(checkNeg(quantityToBuy))
						errorMessageNegative();
					
					else if(quantityToBuy > availableQuantity)
						JOptionPane.showMessageDialog(null, "The quantity you want to buy is more than the available amount");
					
					else if(index == -1)
						JOptionPane.showMessageDialog(null, "No selection has been made");
					
					//if everything is valid
					else
					{
						int newQuantity = availableQuantity - quantityToBuy;
						
						table.setValueAt(newQuantity, index, 4);
						
						JOptionPane.showMessageDialog(null, "Amount owed: " + formatter.format(quantityToBuy*priceOfAlbum));
						
						toBuy.setText("");
						
						
						setLabels(Integer.toString(newQuantity), Double.toString(priceOfAlbum));
						messageOperationSuccess();
						fileChanged();
					}
			
				}
				
			}
			
			//handles the operation to open a file
			else if(ae.getActionCommand().equals("Open"))
			{
				promptSave();
				
				JFileChooser fileChooser = new JFileChooser();
				
				int result = fileChooser.showOpenDialog(null);
				
				grabFile(result, fileChooser);
				Scanner fileIn = null;
				
				try
				{
					fileIn = new Scanner(currentFile);
					
				}catch(FileNotFoundException e){
					System.out.println("File Not Found");
					System.exit(-1);
				}
				
				clearData();
				
				//reads the information from the file and adds onto the table and comboBox
				while(fileIn.hasNextLine())
				{
					int sku = fileIn.nextInt();
					
					fileIn.nextLine();
					
					String artist = fileIn.nextLine();
					String album = fileIn.nextLine();
					double price = fileIn.nextDouble();
					int quantity = fileIn.nextInt();
					
					fileIn.nextLine();
					
					Object[] rowToAdd = {sku, artist, album, formatter.format(price), quantity};
					tableModel.addRow(rowToAdd);
					
					Object elementToAdd = artist = " --- " + album;
					comboBoxModel.addElement(elementToAdd);
				}
				
				fileName = currentFile.getName();
				setTitle("DJ Emporium --- " + fileName);
			}
			
			//handles the operation to Save 
			else if(ae.getActionCommand().equals("Save"))
			{
				saveFile(currentFile);
			}
			
			//handles the operation to remove all data from the 
			else if(ae.getActionCommand().equals("Close"))
			{
				promptSave();
				
				clearData();
			}
			
			//handles the operation to exit
			else
			{
				promptSave();
				
				System.exit(-1);
			}
			
		}

		
		//runs the GUI
		public static void main(String[] args)
		{
			new Qureshi_UberMegaDJStore();
		}

	}
