# GatesManor
Text Based Game for csc122
import java.awt.*;
import java.awt.event.*;
import java.awt.image.*;
import javax.swing.*;
import javax.imageio.*;
import java.io.File;
import java.util.ArrayList;
import java.util.Map;

/*
 * A simple sample game to show how to create GUIs and process text-based user input.
 * 
 * Last updated: 2015-04-15 (tax day!)
 * Original author: Dr. Jean Gourd
 * Author: *****
 */ 
public class GatesManor
{
    private static final String GAME_TITLE = "Gate's Manor";

    // the GUI components
    private JFrame frame;
    private JPanel content;
    private JLabel image_pane;
    private BufferedImage image;
    private JTextArea text_pane;
    private JTextField input_pane;

    // state
    private Room current_room;
    private ArrayList<String> inventory;
    // Room stuff that I am adding
    //Room room2 = new Room("Room 2", "images/room2.jpg", true);
    //ACM changed "room1" into "entrance"
    Room entrance = new Room("Entrance Room", "images/room1.jpg", "You are trapped in a manor for reasons that nobody cares about.  To escape, you must collect the four crest pieces and place them in this room.  This room currently contains crest1, crest2, crest3, and crest4 for testing.", false);
    Room room2 = new Room("Room 2", "images/room2.jpg", "Hello", true);
    Room room3 = new Room("Room 3", "images/room3.jpg", "Hello", true);
    Room room4 = new Room("Room 4", "images/room4.jpg", "Hello", true);
    //ACM Rooms
    Room purpleRoom = new Room("Purple Room", "images/purple room.png", "Hello", true);
    Room blueRoom = new Room("Blue Room", "images/blue room.jpg", "Hello", true);
    Room redRoom = new Room("Red Room", "images/red room.jpg", "Be sure you complete the Blue Room before attempting this puzzle.", true);
    Room starRoom =  new Room("Tapestry Puzzle", "images/tapestry1.png", "Thaegan swallows live ravens whole.", true);
    Room lionRoom = new Room("The Lion's Song", "images/song.png", "The lion sings to you:", false);
    Room crest1 = new Room("Crest Chamber", "images/crest1.png", "Congratulations on solving the puzzle.  Here is a piece of the crest:  crest1.", false);
    Room prep1 = new Room("Holy Water Room", "images/holywater.jpg", "That's strange.  This room shouldn't be here.  That door should have brought me out.", true);
    Room prep2 = new Room("Fish Yogurt Room", "images/fishyogurt.jpg", "This isn't the end yet?  Well, the Kraken should still be weakened.  I can take a breather here.", true);
    Room prep3 = new Room("Gun Room", "images/ak47.jpg", "Still not out yet.  I should take another breather here.", true);
    Room prep4 = new Room("Chain Room", "images/chains.jpg", "I'm not sure I can go on much longer.  This may be my last breather.", true);
    Room krakenChamber = new Room("Kraken's Chamber", "images/boss1.jpg", "A Kraken!  This terrifying beast must be what trapped me here!  Good thing I picked up this holy water.", true);
    Room chamberDepths = new Room("Chamber Depths", "images/boss2.jpg", "The Kraken's back!  Hopefully the fish yogurt will do its job.", true);
    Room conquest = new Room("Room of Conquest", "images/boss3.jpg", "I hope this gun works!", true);
    Room conquer = new Room("Room of Conquer", "images/boss4.jpg", "I won't lose to the Kraken!", true);
    Room victory = new Room("Victory!", "images/victory.jpg", "You have won!", true);
    //ACM Rooms
    
    //JL rooms
    Room greenRoom = new Room("Green Room", "images/green room.jpg", "A green room. Looks like there's two boxes on a table, one with a skull the other a cross. A book rests open on a corner desk.", false);
    Room whiteRoom = new Room("White Room", "images/room1.jpg", "A white room. A large glass case is on the center far wall. Two buttons are on either side. There also appears to be a grate on the floor.", true);
    //JL end
    
    // constructor
    public GatesManor()
    {
        // initialize an empty inventory, add the rooms, make the GUI components, set the current room, and start the game
        inventory = new ArrayList<String>();
        addRooms();
        makeFrame();
        setRoom();
        startGame();
    }

    // adds the rooms
    
    /*
     * test
    private void addPuzzle()
    {
        Puzzle tapestry1, tapestry2, tapestry3, lionstatue;
        
        tapestry1 =  new Puzzle("Tapestry Puzzle 1", "images/tapestry1.png", true);
    }
    */
   
    private void addRooms()
    {
        //Room entrance, room2, room3, room4;
        
        // set the names and images
        //entrance = new Room("Entrance Room", "images/entrance.jpg", false);
        //room2 = new Room("Room 2", "images/room2.jpg", true);
        //room3 = new Room("Room 3", "images/room3.jpg", true);
        //room4 = new Room("Room 4", "images/room4.jpg", true);
        

       
        // add the exits, items, and grabbables
        entrance.addExit("east", room2);
        entrance.addExit("south", room3);
        entrance.addGrabbable("key");
        entrance.addItem("chair", "It is made of wicker and no one is sitting on it.");
        entrance.addItem("table", "It is made of oak.  On it rests a golden key, a redbook, and a bluebook.");
        //ACM Entrance Additions
        entrance.addExit("purple", purpleRoom);
        entrance.addExit("frontdoor", prep1);
        entrance.addGrabbable("redbook");
        entrance.addGrabbable("bluebook");
        entrance.addGrabbable("crest1");
        entrance.addGrabbable("crest2");
        entrance.addGrabbable("crest3");
        entrance.addGrabbable("crest4");
        //ACM Entrance Additions end
        //JL
        entrance.addExit("green", greenRoom);
        //jl
        //ACM Puzzle Rooms
        purpleRoom.addExit("entrance", entrance);
        purpleRoom.addExit("blue", blueRoom);
        purpleRoom.addExit("red", redRoom);
        purpleRoom.addItem("bookshelf", "It is made of oak.  There appears to be room for two more books on it.");
        
        blueRoom.addExit("purple", purpleRoom);
        blueRoom.addExit("star", starRoom);
        blueRoom.addExit("moon", null);
        blueRoom.addItem("candles", "They're made of white wax.  They don't appear to be lit.");
        blueRoom.addItem("fireplace", "Upon closer inspection, the fireplace is actually fake.   It does let off warmth, though.");
        blueRoom.addItem("tapestry", "A very nice tapestry with an interesting design.  In front of it are a star and a moon.");
        blueRoom.addGrabbable("star");
        blueRoom.addGrabbable("moon");
        
        starRoom.addExit("blue", blueRoom);
        starRoom.addGrabbable("bluebook");
        
        redRoom.addExit("purple", purpleRoom);
        redRoom.addExit("lion", lionRoom);
        redRoom.addItem("lamp", "A very nice lamp.  It doesn't appear to work, though.");
        redRoom.addItem("fireplace", "It's covered in a black iron grate.  It seems too tough to remove.");
        redRoom.addItem("couch", "It's very soft and comfortable.  It's a little dusty, though.");
        redRoom.addItem("painting", "It has an interesting design, but you can't really make out what it's supposed to be.");
        redRoom.addItem("table", "There's a cushion on it that makes it seem more like a footrest than a table.");
        redRoom.addItem("pillows", "They're soft, comfortable, and dusty, just like the couch they're on.");
        redRoom.addItem("desklamp", "There's a desk lamp on a stand.  It seems to work just fine.");
        redRoom.addItem("bookshelf", "It's messily covered in a bunch of books.  None of them seem to be very interesting.");
        redRoom.addItem("window", "You want to break through it, but on closer inspection, it appears to be fake.");
        redRoom.addItem("lionstatue", "A statue of a ferocious lion.  Behind it appears to be a lionkey (whatever that is).");
        redRoom.addGrabbable("lionkey");
        
        lionRoom.addExit("red", redRoom);
        lionRoom.addExit("124", null);
        lionRoom.addExit("125", null);
        lionRoom.addExit("126", crest1);
        lionRoom.addExit("127", null);
        lionRoom.addGrabbable("redbook");
        
        crest1.addExit("red", redRoom);
        crest1.addItem("crest1", "It's one of the pieces of the crest!");
        crest1.addGrabbable("crest1");
        crest1.addGrabbable("redbook");
        //ACM Puzzle Rooms end
        
        room2.addExit("west", entrance);
        room2.addExit("south", room4);
        room2.addItem("rug", "It is nice and Indian.  It also needs to be vacuumed.");
        room2.addItem("fireplace", "It is full of ashes.");
        
        room3.addExit("north", entrance);
        room3.addExit("east", room4);
        room3.addGrabbable("book");
        room3.addItem("bookshelves", "They are empty.  Go figure.");
        room3.addItem("statue", "There is nothing special about it.");
        room3.addItem("desk", "The statue is resting on it.  So is a book.");
        
        room4.addExit("north", room2);
        room4.addExit("west", room3);
        room4.addExit("south", null);
        room4.addGrabbable("6-pack");
        room4.addItem("brew_rig", "Gourd is brewing some sort of oatmeal stout on the brew rig.  A 6-pack is resting beside it.");
        
        //ACM Boss Rooms
        prep1.addExit("entrance", entrance);
        prep1.addExit("chamber", krakenChamber);
        prep1.addItem("holywater", "I should probably bring that with me.  I have a bad feeling about what's ahead...");
        prep1.addGrabbable("holywater");
        
        krakenChamber.addExit("escape", prep2);
        
        prep2.addExit("chamber", krakenChamber);
        prep2.addExit("depths", chamberDepths);
        prep2.addItem("fishyogurt", "It's a bowl fish yogurt.  It is common knowledge that Krakens love this stuff.  I can use it as a distraction!");
        prep2.addGrabbable("fishyogurt");
        
        chamberDepths.addExit("escape2", prep3);
        
        prep3.addExit("depths", chamberDepths);
        prep3.addExit("conquest", conquest);
        prep3.addItem("ak47", "For some reason, I don't think it'll do much to the Kraken, but it doesn't hurt to try.");
        prep3.addGrabbable("ak47");
        
        conquest.addExit("escape3", prep4);
        
        prep4.addExit("conquest", conquest);
        prep4.addExit("conquer", conquer);
        prep4.addItem("honorchains", "Some honor chains!  These are perfect!  The Kraken must have been trapped in these, but broke out.  I can use these to rebind it!");
        prep4.addGrabbable("honorchains");
        
        conquer.addExit("escape4", victory);
        //ACM Boss Rooms end
        
          //JL Rooms
        greenRoom.addExit("entrance", entrance);
        greenRoom.addExit("white", whiteRoom);
        greenRoom.addGrabbable("whitekey");
        greenRoom.addItem("book", "Written inside in some awful handwriting is 'Choose the right button'");
        greenRoom.addItem("whitebox", "It's a white box with a cross on the lid, inside is a white key.");
        greenRoom.addItem("blackbox", "A black box nailed to the table. It has some fancy engraving of a skull on the lid. Inside seems to be a skull button. I mean, I could press it if I wanted to.");
        
        whiteRoom.addExit("green", greenRoom);
        whiteRoom.addItem("glass_case", "There's a large glass case in the center of the far wall. A piece of a crest lies inside.");
        whiteRoom.addItem("buttons", "Two buttons are on either side of the glass case. One on the left and one on the right. They seem to be identical. I should try pressing one. ");
        whiteRoom.addGrabbable("crest2");
        
        //JL Rooms end
        
        
        
        
        
        
        // set the current room (entrance initially)
        current_room = entrance;
    }
    
    // sets the current room (image and text)
    private void setRoom()
    {
        // set the description
        setDescription("");
        
        // set the image
        try
        {
            // death means the skull
            if (current_room == null)
            {
                image = ImageIO.read(new File("images/skull.jpg"));
            }
            // otherwise, load the appropriate room image
            /*else if(current_room.equals(room2))
            {
              if(inventory.contains("key")== true)
              {
                 image = ImageIO.read(new File(current_room.getImage())); 
                                   
              } 
              else
              {
               setDescription("The door is locked");   
              }
                
            } */
            else
            {
                image = ImageIO.read(new File(current_room.getImage()));
            }
            
            image_pane.setIcon(new ImageIcon(image));
        }
        catch (Exception e)
        {}        
        
        // pack the frame so the new image is rendered and the window is resized
        frame.pack();
    }
    
    // set the room description
    private void setDescription(String s)
    {
        // death means death
        if (current_room == null)
        {
            text_pane.setText("You are dead.");
        }
        else
        {
            text_pane.setText(current_room + "\nYou are carrying: " + inventory + "\n\n" + s);
        }
    }
    
    // starts the game
    private void startGame()
    {
        Dimension d = Toolkit.getDefaultToolkit().getScreenSize();
        
        // position the window in the center of the screen, make it visible, and give the input_pane focus
        frame.setLocation(d.width / 2 - frame.getWidth() / 2, d.height / 2 - frame.getHeight() / 2);
        frame.setVisible(true);
        input_pane.requestFocus();
    }
    
    // makes the GUI components
    private void makeFrame()
    {
        // the frame and main content pane
        frame = new JFrame(GAME_TITLE);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        content = (JPanel)frame.getContentPane();
        
        // the image pane
        image_pane = new JLabel();
        image_pane.setBorder(BorderFactory.createMatteBorder(1, 1, 1, 1, Color.BLACK));
        content.add(image_pane, BorderLayout.LINE_START);
        
        // the text pane
        text_pane = new JTextArea(20, 19);
        text_pane.setBorder(BorderFactory.createMatteBorder(1, 0, 1, 1, Color.BLACK));
        text_pane.setEditable(false);
        text_pane.setLineWrap(true);
        text_pane.setFont(new Font("Arial", 1, 20));
        content.add(text_pane, BorderLayout.LINE_END);
        
        /// the input pane
        input_pane = new JTextField();
        input_pane.setFont(new Font("Courier new", 1, 20));
        input_pane.setColumns(35);
        input_pane.setBorder(BorderFactory.createMatteBorder(0, 1, 1, 1, Color.BLACK));
        content.add(input_pane, BorderLayout.PAGE_END);
        
        // add a listener to the input pane so that actions can occur when the user presses enter
        input_pane.addActionListener(new ActionListener()
        {
            public void actionPerformed(ActionEvent e)
            {
                // process the input and clear the input pane
                process(input_pane.getText());
                input_pane.setText("");
            }
        });
    }
    
    // process user input from the input pane
    private void process(String s)
    {
        String sl = s.toLowerCase().trim(); // the lowercase version of the user input
        String[] words;                     // user input split into words
        String verb;                        // the specified verb
        String noun;                        // the specified noun
        String response = "I don't understand.  Try:\n<verb> <noun>\nValid <verb>: go look take place";
        
        // handle quitting
        if (sl.equals("quit") || sl.equals("exit") || sl.equals("bye"))
        {
            System.exit(0);
        }
 
        // if we've already died, simply get out of here
        // this allows the user to still quit, exit, or bye above
        if (current_room == null)
        {
            return;
        }
        
        // split the input into words
        words = sl.split(" ");
        
        // only accept two words (verb and noun)
        if (words.length == 2)
        {
            verb = words[0];
            noun = words[1];

            // act based on the verb
            if (verb.equals("go"))
            {
                // set a default response
                response = "Invalid exit.";
                if(noun.equals("east"))
                {
                    if(inventory.contains("key"))
                    {
                        room2.unLocked(false);
                        for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                        {
                            // get the exit
                            String exit = entry.getKey();
                           
                            // if it's the one chosen
                            if (noun.equals(exit))
                            {
                                // /change the current room and set it on the GUI
                                current_room = entry.getValue();
                                setRoom();
                                response = "";
                                
                                break;
                            }
                        }
                    }
                    if(room2.isLocked() == true){
                        response = "The Door is locked";
                        
                        
                    }
                }
                //ACM Room Locks
                else if(noun.equals("lion"))
                {
                    if(inventory.contains("lionkey"))
                    {
                        lionRoom.unLocked(false);
                        for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                        {
                            // get the exit
                            String exit = entry.getKey();
                           
                            // if it's the one chosen
                            if (noun.equals(exit))
                            {
                                // /change the current room and set it on the GUI
                                current_room = entry.getValue();
                                setRoom();
                                response = "";
                                
                                break;
                            }
                        }
                    }
                    if(lionRoom.isLocked() == true){
                        response = "The Door is locked";
                        
                        
                    }
                }
                else if(noun.equals("blue"))
                {
                    for (String grabbable : current_room.getGrabbables())
                    {
                        // if it's the one chosen
                        if (grabbable.equals("bluebook"))
                        {
                            blueRoom.unLocked(false);
                            for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                            {
                                // get the exit
                                String exit = entry.getKey();
                                
                                // if it's the one chosen
                                if (noun.equals(exit))
                                {
                                    // /change the current room and set it on the GUI
                                    current_room = entry.getValue();
                                    setRoom();
                                    response = "";
                                    
                                    break;
                                }
                            }
                        }
                    }
                    if(blueRoom.isLocked() == true)
                    {
                        response = "The Door is locked";
                    }
                }
                else if(noun.equals("star"))
                {
                    if(inventory.contains("star"))
                    {
                        starRoom.unLocked(false);
                        for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                        {
                            // get the exit
                            String exit = entry.getKey();
                           
                            // if it's the one chosen
                            if (noun.equals(exit))
                            {
                                // /change the current room and set it on the GUI
                                current_room = entry.getValue();
                                setRoom();
                                response = "";
                                
                                break;
                            }
                        }
                    }
                    if(starRoom.isLocked() == true)
                    {
                        response = "The Door is locked";
                    }
                }
                else if(noun.equals("red"))
                {
                    for (String grabbable : current_room.getGrabbables())
                    {
                        if(grabbable.equals("redbook"))
                        {
                            redRoom.unLocked(false);
                            for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                            {
                                // get the exit
                                String exit = entry.getKey();
                                
                                // if it's the one chosen
                                if (noun.equals(exit))
                                {
                                    // /change the current room and set it on the GUI
                                    current_room = entry.getValue();
                                    setRoom();
                                    response = "";
                                    
                                    break;
                                }
                            }
                        }
                    }
                    if(redRoom.isLocked() == true)
                    {
                        response = "The Door is locked"; 
                    }
                }
                else if(noun.equals("frontdoor"))
                {
                    if((inventory.contains("crest1")) && (inventory.contains("crest2")) && (inventory.contains("crest3")) && (inventory.contains("crest4")))
                    {
                        prep1.unLocked(false);
                        for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                        {
                            // get the exit
                            String exit = entry.getKey();
                           
                            // if it's the one chosen
                            if (noun.equals(exit))
                            {
                                // /change the current room and set it on the GUI
                                current_room = entry.getValue();
                                setRoom();
                                response = "";
                                
                                break;
                            }
                        }
                    }
                    if(prep1.isLocked() == true){
                        response = "I need the four pieces of the crest to get through.";
                        
                        
                    }
                }
                else if(noun.equals("chamber"))
                {
                    if(inventory.contains("holywater"))
                    {
                        krakenChamber.unLocked(false);
                        for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                        {
                            // get the exit
                            String exit = entry.getKey();
                           
                            // if it's the one chosen
                            if (noun.equals(exit))
                            {
                                // /change the current room and set it on the GUI
                                current_room = entry.getValue();
                                setRoom();
                                response = "";
                                
                                break;
                            }
                        }
                    }
                    if(krakenChamber.isLocked() == true){
                        response = "I should look around this room a bit first.";
                        
                        
                    }
                }
                else if(noun.equals("depths"))
                {
                    if(inventory.contains("fishyogurt"))
                    {
                        chamberDepths.unLocked(false);
                        for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                        {
                            // get the exit
                            String exit = entry.getKey();
                           
                            // if it's the one chosen
                            if (noun.equals(exit))
                            {
                                // /change the current room and set it on the GUI
                                current_room = entry.getValue();
                                setRoom();
                                response = "";
                                
                                break;
                            }
                        }
                    }
                    if(chamberDepths.isLocked() == true){
                        response = "I should look around this room a bit first.";
                        
                        
                    }
                }
                else if(noun.equals("conquest"))
                {
                    if(inventory.contains("ak47"))
                    {
                        conquest.unLocked(false);
                        for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                        {
                            // get the exit
                            String exit = entry.getKey();
                           
                            // if it's the one chosen
                            if (noun.equals(exit))
                            {
                                // /change the current room and set it on the GUI
                                current_room = entry.getValue();
                                setRoom();
                                response = "";
                                
                                break;
                            }
                        }
                    }
                    if(conquest.isLocked() == true){
                        response = "I should look around this room a bit first.";
                        
                        
                    }
                }
                else if(noun.equals("conquer"))
                {
                    if(inventory.contains("honorchains"))
                    {
                        conquer.unLocked(false);
                        for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                        {
                            // get the exit
                            String exit = entry.getKey();
                           
                            // if it's the one chosen
                            if (noun.equals(exit))
                            {
                                // /change the current room and set it on the GUI
                                current_room = entry.getValue();
                                setRoom();
                                response = "";
                                
                                break;
                            }
                        }
                    }
                    if(conquer.isLocked() == true){
                        response = "I should look around this room a bit first.";
                        
                        
                    }
                }
                else if(noun.equals("escape"))
                {
                    if((!inventory.contains("holywater")) && (!inventory.contains("crest1")))
                    {
                        prep2.unLocked(false);
                        for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                        {
                            // get the exit
                            String exit = entry.getKey();
                            
                            // if it's the one chosen
                            if (noun.equals(exit))
                            {
                                // /change the current room and set it on the GUI
                                current_room = entry.getValue();
                                setRoom();
                                response = "";
                                
                                break;
                            }
                        }
                    }
                    if(prep2.isLocked() == true)
                    {
                        if((inventory.contains("crest1")) && (!inventory.contains("holywater")))
                        {
                            response = "There's a hole in the shape of crest1.  I should place it.";
                        }
                        else
                        {
                            response = "I should try to 'throw holywater' at the Kraken.";
                        }
                    }
                }
                else if(noun.equals("escape2"))
                {
                    if((!inventory.contains("fishyogurt")) && (!inventory.contains("crest2")))
                    {
                        prep3.unLocked(false);
                        for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                        {
                            // get the exit
                            String exit = entry.getKey();
                            
                            // if it's the one chosen
                            if (noun.equals(exit))
                            {
                                // /change the current room and set it on the GUI
                                current_room = entry.getValue();
                                setRoom();
                                response = "";
                                
                                break;
                            }
                        }
                    }
                    if(prep3.isLocked() == true)
                    {
                        if((inventory.contains("crest2")) && (!inventory.contains("fishyogurt")))
                        {
                            response = "There's a hole in the shape of crest2.  I should place it.";
                        }
                        else
                        {
                            response = "I should try to 'throw fishyogurt' at the Kraken.";
                        }
                    }
                }
                else if(noun.equals("escape3"))
                {
                    if((!inventory.contains("ak47")) && (!inventory.contains("crest3")))
                    {
                        prep4.unLocked(false);
                        for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                        {
                            // get the exit
                            String exit = entry.getKey();
                            
                            // if it's the one chosen
                            if (noun.equals(exit))
                            {
                                // /change the current room and set it on the GUI
                                current_room = entry.getValue();
                                setRoom();
                                response = "";
                                
                                break;
                            }
                        }
                    }
                    if(prep4.isLocked() == true)
                    {
                        if((inventory.contains("crest3")) && (!inventory.contains("ak47")))
                        {
                            response = "There's a hole in the shape of crest3.  I should place it.";
                        }
                        else
                        {
                            response = "I should try to 'fire ak47' at the Kraken.";
                        }
                    }
                }
                else if(noun.equals("escape4"))
                {
                    if((!inventory.contains("honorchains")) && (!inventory.contains("crest4")))
                    {
                        victory.unLocked(false);
                        for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                        {
                            // get the exit
                            String exit = entry.getKey();
                            
                            // if it's the one chosen
                            if (noun.equals(exit))
                            {
                                // /change the current room and set it on the GUI
                                current_room = entry.getValue();
                                setRoom();
                                response = "";
                                
                                break;
                            }
                        }
                    }
                    if(victory.isLocked() == true)
                    {
                        if((inventory.contains("crest4")) & (!inventory.contains("honorchains")))
                        {
                            response = "There's a hole in the shape of crest4.  I should place it.";
                        }
                        else
                        {
                            response = "I should try to 'use honorchains' on the Kraken.";
                        }
                    }
                }
                //ACM Room Locks end
                //JL Room Locks
                 else if(noun.equals("white"))
                {
                    if(inventory.contains("whitekey"))
                    {
                        whiteRoom.unLocked(false);
                        for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                        {
                            // get the exit
                            String exit = entry.getKey();
                           
                            // if it's the one chosen
                            if (noun.equals(exit))
                            {
                                // /change the current room and set it on the GUI
                                current_room = entry.getValue();
                                setRoom();
                                response = "";
                                inventory.remove("whitekey");
                                break;
                            }
                        }
                    }
                    if(whiteRoom.isLocked() == true)
                    {
                        response = "The Door is locked";
                        
                        
                    }
                }
                
                //JL locks end
                
                
                
                else
                {
                // iterate through the keys and values of exits in the current room
                for (Map.Entry<String,Room> entry : current_room.getExits().entrySet())
                {
                    // get the exit
                    String exit = entry.getKey();
                    
                    // if it's the one chosen
                    if (noun.equals(exit))
                    {
                        // /change the current room and set it on the GUI
                        current_room = entry.getValue();
                        setRoom();
                        response = "";
                        
                        break;
                    }
                }
            }
            
            }
            else if (verb.equals("look"))
            {
                response = "I don't see that item.";
             
                // iterate through the keys and values of the items in the current room
                for (Map.Entry<String,String> entry : current_room.getItems().entrySet())
                {
                    // get the item
                    String item = entry.getKey();
                    
                    // if it's the one chosen
                    if (noun.equals(item))
                    {
                        // set the response to the item's description
                        response = entry.getValue();
                        
                        break;
                    }
                }
            }
            else if (verb.equals("take"))
            {
                response = "I don't see that item.";
                
                // iterate through the grabbables in the current room
                for (String grabbable : current_room.getGrabbables())
                {
                    // if it's the one chosen
                    if (noun.equals(grabbable))
                    {
                        // add it to the inventory
                        inventory.add(grabbable);
                        // remove it from the room's grabbables
                        current_room.removeGrabbable(grabbable);
                        response = "Item grabbed.";
                        
                        break;
                    }
                }
            }
            //JL press
            else if (verb.equals("press"))
            {
                response = "Pressing that doesn't do anything";
                
                if(noun.equals("rightbutton"))
                {
                    response = "The case opened! I can grab the crest piece now!";
                }else if(noun.equals("leftbutton"))
                {   
                    current_room = null;
                    response = "You here the door lock behind. Water begins filling the room from the grate on the floor. This is it. You are RIP.";
                }else if(noun.equals("skullbutton"))
                {
                    current_room = null;
                }
            }
            
            
           //Jl Press end
           //ACM Boss Commands
            else if (verb.equals("place"))
            {
                response = "I shouldn't place that.";
                
                if((noun.equals("holywater")) || (noun.equals("fishyogurt")) || (noun.equals("ak47")) || (noun.equals("honorchains")))
                {
                    response = "I shouldn't place that.";
                }
                else
                {
                    if(inventory.contains(noun))
                    {
                        //remove the item from your inventory
                        inventory.remove(noun);
                        //add the item as a grababble for the room
                        current_room.addGrabbable(noun);
                        response = "Item placed.";
                    }
                }
           }
           else if (verb.equals("throw"))
            {
                response = "I shouldn't throw that.";
                
                if((inventory.contains("holywater")) || (inventory.contains("fishyogurt")))
                {
                    if((noun.equals("holywater")) || (noun.equals("fishyogurt")))
                    {
                        //remove the item from your inventory
                        inventory.remove(noun);
                        if(noun.equals("holywater"))
                        {
                            response = "It seems to have weakened the Kraken!";
                        }
                        else
                        {
                            response = "The Kraken's distracted!";
                        }
                    }
                }
           }
           else if (verb.equals("fire"))
            {
                response = "How am I supposed to fire that?";
                
                if(inventory.contains("ak47"))
                {
                    if(noun.equals("ak47"))
                    {
                        //remove the item from your inventory
                        inventory.remove(noun);
                        response = "You used all the ammo, but it didn't do much so you threw it away.  At least it distracted the Kraken.";
                    }
                }
           }
           else if (verb.equals("use"))
            {
                response = "What should I do with this?";
                
                if(inventory.contains("honorchains"))
                {
                    if(noun.equals("honorchains"))
                    {
                        //remove the item from your inventory
                        inventory.remove(noun);
                        response = "You bound the Kraken!  The Kraken is defeated!";
                    }
                }
           }
        }
        //ACM Boss Commands end
           
       setDescription(response);
   }
}
