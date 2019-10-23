# Unity1-TerminalHacker-Demo

### DO NOT CLONE this demo. This repo has the working solution of the terminal hacker game.  Instead, create a new project and follow these steps:

1. Create a new 2D Unity Project
2. Assets > Import Package > Custom Package [these](https://drive.google.com/open?id=1_0dUHQ9KFmEfOyo2VTyzJjP1_0q5wjiR) assets
3. Open the WM2000 folder, then drag and drop the WM2000 Game Object into the Hierarchy
4. Save the scene. Rename it to `Terminal`.
5. Hit the "Play" button. Explain that the play button runs your game alongside any scrips associated with it. As of now, we did not add any scrips of our own.
6. Inside the Assets folder, add a new script called "Hacker".
7. Open the script, and inside the Start() function, write:

```
		    // Use this for initialization
		    void Start () {
		        Terminal.WriteLine ("What would you like to hack into?");
		        Terminal.WriteLine ("Press 1 for Coded");
		        Terminal.WriteLine("Press 2 the airport");
		        Terminal.WriteLine ("Enter your selection:");
		    }
```
		
8. Run the game. Show that just by adding the script, it doesn't really do anything until we attach it to a game object! Then drag and drop the script onto the WM2000 game object.
9. Move the Terminal.WriteLine into a function. Then, show them how to pass a parameter into the function

```

		    void Start () {
		        ShowMainMenu ("Hello Huss");
		    }
		
		    void ShowMainMenu(string greeting){
		        Terminal.WriteLine (greeting);
		        Terminal.WriteLine ("What would you like to hack into?");
		        Terminal.WriteLine ("Press 1 for Coded");
		        Terminal.WriteLine ("Press 2 for the airport");
		        Terminal.WriteLine ("Enter your selection:");
		    }
```
		   
		
10. Lose a semi colon somewhere to show them how to read errors
------------------------------------------------------------------
GIVE TASK [Hack the Planet 1](https://warehouse.joincoded.com/assignments/tasks/unity/03-hack-the-planet-1-main-menu/) Now
------------------------------------------------------------------
11. Use the OnUserInput (build into this project, not a Unity thing), and show them print statement in console
```
		    void OnUserInput(string input){
		        print (input == "1");
		    }
```
		
12. Add a level variable and an enum up top (enum is to limit our options, and to help reduce bugs)
```
		    //Game state
		    int level;
		    enum Screen {MainMenu, Password, Win};
		    Screen currentScreen = Screen.MainMenu;
```
		
13. Refactoring to make things look more organized:

```
		    void SetLevel(string selectedLevel){
		        if (selectedLevel == "1") {
		            level = 1;
		        } else if (selectedLevel == "2") {
		            level = 2;
		        }
		    }
		    
		   void OnUserInput(string input){
		        if (input == "menu") {
		            ShowMainMenu ("Welcome back, Agent Huss");
		        } else {
		            SetLevel(input);
		        }
		    }
```

This works, but the user has no feedback. Write a `StartGame()` method to display some user feedback, and call it in `SetLevel()`:

```
		    void StartGame() {
		    	currentScreen = Screen.Password;
		        Terminal.WriteLine ("You have chosen level " + level);
		    }
		    
		    void SetLevel(string selectedLevel){
		        if (selectedLevel == "1") {
		            level = 1;
			    StartGame()
		        } else if (selectedLevel == "2") {
		            level = 2;
			    StartGame()
		        }
		    }
		   
```
When typing "menu", the main menu is displayed again, but the screen is not cleared. Use `Terminal.ClearScreen ();` first thing inside the `ShowMainMenu()` method. 

--------------------------------------------------------------------------------
GIVE TASK [Hack the Planet 2](https://warehouse.joincoded.com/assignments/tasks/unity/04-hack-the-planet-2-functions/) NOW
--------------------------------------------------------------------------------
14. Add a string array of level1Passwords and level2Passwords
```
		    string[] level1Passwords = {"projector", "monitor", "mouse", "keyboard"};
		    string[] level2Passwords = {"airbus", "terminal", "gate", "baggage"}; 
```
		
15. Add a string `password` to the Game State, and give it a value `test`
16. Add a new condition to the `OnUserInput` method

```
		    void OnUserInput(string input){
		        if (input == "menu") {
		            ShowMainMenu ("Welcome back, Agent Huss");
		        } else if (currentScreen == Screen.MainMenu) {
		             SetLevel(input);
		        } else if (currentScreen == Screen.Password) {
		            CheckPassword(input);
		        }
		    } 
```

17. Add the CheckPassword method
```
		    void CheckPassword(string input){
		        if (input == password) {
		            Terminal.WriteLine("YOU WIN!")
		
		        } else {
		            Terminal.WriteLine("WRONG PASSWORD! Guess again or type menu to go back");
		        }
		    }		  
```

Now, run the game to show what happens when there is a password match, and when there is no match. 
		 
18. In start game, use a switch statement to set the password

```
		    void StartGame() {
		        currentScreen = Screen.Password;
		        Terminal.WriteLine ("You have chosen level " + level);
		        switch(level) {
		        case 1: 
		            password = level1Passwords[2]; //make random later on
		            break;
		        case 2:
		            password = level2Passwords[2];
		            break;
		        default:
		            Debug.LogError("Invalid level number");
		            break;
		        }
		    } 
```
		     
		
19. Make the password randomized. On the white board, draw a diagram to show the array, and how we are selecting a random value out of this array.

```
	        case 1: 
	            password = level1Passwords [Random.Range(0, level1Passwords.Length)]; //make random later on
	            print(password);
	            break;
	        case 2:
	            password = level2Passwords [Random.Range(0, level2Passwords.Length)];
	            break;
		    
```

20. Refactor to make code simpler

```

    void StartGame() {
        AskForPassword();
    }

    void CheckPassword(string input){
        if (input == password) {
            DisplayWinScreen();

        } else {
            Terminal.WriteLine("You shall not pass!");
        }
    }

    void AskForPassword(){
        currentScreen = Screen.Password;
        Terminal.WriteLine ("You have chosen level " + level);
        SetRandomPassword ();
        Terminal.WriteLine ("Enter password: " + password.Anagram()); //Tell them that this method is predefined. Use it. You don't need to know how it works, just what it does.
    }

    void SetRandomPassword(){
        switch(level) {
        case 1: 
            password = level1Passwords [Random.Range(0, level1Passwords.Length)]; //make random later on
            break;
        case 2:
            password = level2Passwords [Random.Range(0, level2Passwords.Length)];
            break;
        default:
            Debug.LogError("Invalid level number");
            break;
        }
    }
    
    void DisplayWinScreen(){
		currentScreen = Screen.Win;
		Terminal.ClearScreen();
		Terminal.WriteLine("YOU WON!");
	}
 ```
 
--------------------------------------------------------------------------------
GIVE TASK [Hack the Planet 3]() NOW
--------------------------------------------------------------------------------
