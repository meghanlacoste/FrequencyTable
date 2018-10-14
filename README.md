# FrequencyTable
package com.company;
import java.util.*;
import java.io.*;
import static com.company.ProjConstants.*;
public class Main {

    public static void main(String[] args) {
        // write your code here

        // Create 2- dimensional String (aka. matrix) that Stores Strings and counts frequency (arrayStoreFreq)
        String arrayStoreFreq[][] = new String [MAXDATA][2];



        // Initialize the array to a known value
        // - loops over each row using "i", and columns using "j"
        // -  sets all array values to INVALID

        for(int i=0; i < MAXDATA; i++) {
            for(int j = 0; j < 2; j++){
                arrayStoreFreq[i][j] = "invalid";
            }
        }


        // INITIALIZE VARIABLES
        String userFileName;
        boolean checkUnique = false;


        System.out.println("");
        System.out.println("Please type in the file name\n");

        // create a variable s of type scanner to process input from "System.in"
        Scanner scanSystemIn = new Scanner(System.in);

        // Use the Scanner "s" to get the "next" input from "System.in"
        userFileName = scanSystemIn.next();

        // Display the user input now stored in "userInput"
        System.out.println("\nThe user input: " + userFileName);


        // ---------------------------------------------
        // This is where we "try" to process the file
        //

        try {
            // --------------------------------
            // create file, and scanner objects
            // - file object is called tempfilenums.txt and is in your project directory
            //   that is the same folder as the iml file
            //

            File userFile = new File(userFileName);
            Scanner scanUserFile = new Scanner(userFile);

            // ---------------------------------------------
            // Reads in values from the file in a for loop
            //

            for(int i= 0; i < MAXDATA; i++) {

                // ---------------------------------------------
                // The scanner checks if there is another string
                //

                if (scanUserFile.hasNext()) {

                    // converts string to lower case

                   String userString =  scanUserFile.next().toLowerCase();




                  //  while (!checkUnique) {

                        for (int unique = 0; unique <= i; unique++) {

                            //Checks if string is replicated



                            if (userString.equalsIgnoreCase(arrayStoreFreq[unique][0])) {



                                // - add condition  && (userString!= "invalid")?

                                //  ADD ONE TO FREQUENCY\

                                // arrayStoreFreq[i][0]= "invalid";

                                System.out.println("\n replicated: " + userString + " " + arrayStoreFreq[unique][0] + " " + unique);

                               // checkUnique = true;

                                // break;

                            } else {

                                arrayStoreFreq[i][1] = "one";

                                arrayStoreFreq[i][0] = userString;

                                System.out.println(arrayStoreFreq[i][0] + " " + i);

                               // checkUnique = true;

                              //   break;
                            }


                        }// end for (int unique = MAXDATA-1; unique>=0; unique--){

                    //} // end while

                    } else {
                    // ---------------------------------------------
                    // The scanner detected no other integers
                    // - closes the scanner for the file
                    // - breaks out of the for loop
                    //
                    System.out.print("\n\nDataFile has been completely READ");
                    scanUserFile.close();

                    // A break statement allows us to exit the loop before we have reach the end
                    break;
                }
            }

            System.out.println();
            for (int i = 0; i < MAXDATA; i++) {
                if (arrayStoreFreq[i][0] != "invalid")
                    System.out.println(arrayStoreFreq[i][0] + " " + i);
            }

            // ---------------------------------------------
            // If the file cannot be found then an exception (error) is generated (thrown) that we have to
            // deal with (catch).
            // - we print "e" the exception, and
            // - show the user where it was using the "exceptions" stack trace information
            //

        } catch (FileNotFoundException e) {
            System.out.println(e);
            e.printStackTrace();
        }




    }// END MAIN METHOD
}// END MAIN CLASS
/*
Import Java utilities

Main Class

Main Method


Create 2- dimensional String (aka. matrix) that Stores Strings and counts frequency (arrayStoreFreq)

Initialize array to set value by looping over the int 'i' (representing rows) and 'j' (representing columns).

Method STORE
If system input is a new word (does not match strings of previous indexes) Then
-  set to lowercase and store the string in the next index position in the first column of the matrix
Else
   - add 1 to the frequency of the value at index where string is stored


Declare a variable of String type for user input (String userFileName)
Request user for name of file to be read
Create a variable 's' which is a scanner to process input from "System.in"

Use the Scanner "s" to get the "next" input from "System.in" and set that input to String userFileName

Display String userFileName


TRY to process file
 Create file, and scanner objects
 Reads in values from the file in a for loop

	For (int i=0; i < Max , i ++; )
	If scanner detects another String in the file Then
		Call upon Method STORE
Else The scanner detected no other Strings
       - close the scanner for the file
       - break out of the for loop
       - display that file was completely read to the screen

End For-loop

 For ( int i =0; i < arrayStoreFreq.length; i++)
Print value at index position 'i' for the String Stored and
then for the frequency of that String -- displaying it neatly to the screen

CATCH
  If the file cannot be found then an exception is generated (thrown)
  Print  "e" the exception, and show the user where it was using the "exceptions" stack trace information

Create second 2-D array called Substrings that consists of 3 columns
	Loop over data stored in arrayStoreFreq
		If String at one index position equals a substring at another index position
			- add 1 to the frequency of that index in column 2
- add string to list of that index in column 3

	End Loop


 For ( int j =0; j < arrayStoreFreq.length; j++)
-Print String at index position 'j' for the column 3 and then for the frequency of that substring-- displaying it neatly to the screen




 */
