
package com.company;

import static com.company.ProjConstants.*;
import java.io.*;
import java.util.*;

public class Main {

    private static void displayResults(int numItems, double ave, double var, double dev){

        System.out.println("==========================================================================================");
        System.out.println("                       Data Distribution Info\n");
        System.out.printf("\tNumber of Items........ %10.0f\n", (double) numItems);
        System.out.printf("\tAverage:............... %10.4f\n", ave);
        System.out.printf("\tVariance:.............. %10.4f\n", var);
        System.out.printf("\tStandard Deviation:.... %10.4f\n", dev);
        System.out.println("\n                       Range Values\n");
        System.out.printf("\t68 %% of Data:\t %10.4f    <  %10.4f    <  %10.4f\n", (ave - dev),ave,(ave + dev));
        System.out.printf("\t95 %% of Data:\t %10.4f    <  %10.4f    <  %10.4f\n", (ave - (2*dev)),ave,(ave + (2*dev)));
        System.out.printf("\t99 %% of Data:\t %10.4f    <  %10.4f    <  %10.4f\n", (ave - (3*dev)),ave,(ave + (3*dev)));
        System.out.println("==========================================================================================\n");
    }

    public static void main(String[] args) {

        boolean rangeSet          = false;
        boolean methodSelected    = false;
        boolean userExitRequest   = false;
        boolean dataError         = false;

        String  userInputFileName = null;

        int     howManyFileItems  = INVALID;
        int     howManyDataItems  = INVALID;
        int     methodSelection   = INVALID;
        int     rangeValue        = INVALID;

        double  theAverage        = INVALID;
        double  theVariance       = INVALID;
        double  theStDeviation    = INVALID;

        StDeviation CalcSD = new StDeviation();

        Scanner scanSystemIn = new Scanner(System.in, "UTF-8");

        while (!methodSelected){

            System.out.println("==========================================================================================");
            System.out.println("\t Please select the Calculation Method");
            System.out.println("\t\t 1) Discrete Values");
            System.out.println("\t\t 2) Discrete Values in a Frequency Table");
            System.out.println("\t\t 3) Discrete/Continuous Values in Groups");
            System.out.println("\t\t 4) Exit");

            if (scanSystemIn.hasNextInt()){

                methodSelection = scanSystemIn.nextInt();

                switch (methodSelection){
                    case 1:{ // Discrete Values
                        CalcSD.setCalcMethod(DISCRETE);
                        methodSelected = true;
                        break;
                    }

                    case 2:{ // Discrete Values in Frequency Table
                        CalcSD.setCalcMethod(FRQTABLE);
                        methodSelected = true;

                        while (!rangeSet){
                            System.out.println("\t Please enter the Minimum Value for the Range");
                            if (scanSystemIn.hasNextInt()) {

                                rangeValue = scanSystemIn.nextInt();
                                CalcSD.setMin(rangeValue);

                                System.out.println("\t Please enter the Maximum Value for the Range");
                                if (scanSystemIn.hasNextInt()) {

                                    rangeValue = scanSystemIn.nextInt();
                                    CalcSD.setMax(rangeValue);

                                    rangeSet = true;
                                }
                                else {
                                    rangeSet = false;
                                }
                            }
                            else {
                                rangeSet = false;
                            }
                        }
                        break;
                    }

                    case 3: { // Grouped Values

                            CalcSD.setCalcMethod(GROUPED);
                            methodSelected = true;

                            while (!rangeSet) {
                                System.out.println("Please enter the MINIMUM VALUE in your file");
                                if (scanSystemIn.hasNextInt()) {

                                    rangeValue = scanSystemIn.nextInt();
                                    CalcSD.setMin(rangeValue);

                                    System.out.println("Please enter the MAXIMUM VALUE in your file");
                                    if (scanSystemIn.hasNextInt()) {

                                        rangeValue = scanSystemIn.nextInt();
                                        CalcSD.setMax(rangeValue);

                                        rangeSet= true;

                                        System.out.println("Enter the NUMBER OF GROUPS desired:");
                                        if (scanSystemIn.hasNextInt()) {
                                           rangeValue = scanSystemIn.nextInt();
                                           CalcSD.setNumberOfGroups(rangeValue);
                                            System.out.print("number of groups "+ CalcSD.getNumberOfGroups());
                                        } else{
                                            rangeSet = false;
                                        }

                                    } else {
                                        rangeSet = false;
                                    }
                                } else {

                                    rangeSet = false;
                                }
                            }

                            break;
                    }


                    case 4:{ // Exit Request
                        userExitRequest = true;
                        methodSelected = true;
                        break;
                    }

                    default:{ // Error Case
                        System.out.println("\t Please Enter a valid option");
                        methodSelected = false;
                        break;
                    }
                }
            }
            else {
                System.out.println("\t Please Enter only integer values");
                methodSelected = false;
                scanSystemIn.next();
            }
        }

        if (!userExitRequest){

            System.out.println("\nPlease type in the INPUT file name\n");
            userInputFileName = scanSystemIn.next();

            try {

                File userFile = new File(userInputFileName);
                Scanner scanUserFile = new Scanner(userFile, "UTF-8");

                howManyFileItems = 0;

                methodSelection  = CalcSD.getCalcMethod();

                while ((scanUserFile.hasNext()) && (!dataError)){

                    howManyFileItems++;

                    switch (methodSelection) {

                        case DISCRETE: {
                            if (howManyFileItems <= MAXDATA){
                                CalcSD.addNewDataItem(scanUserFile.nextInt());
                            }
                            else {
                                System.out.println("\nWARNING: Discrete Calculation Method, data in file exceeds storage capacity\n");
                                dataError = true;
                            }
                            break;
                        }

                        case FRQTABLE: {
                            CalcSD.addNewDataItem(scanUserFile.nextInt());
                            break;
                        }

                        case GROUPED: {
                            CalcSD.addNewDataItem(scanUserFile.nextInt());
                            break;
                        }

                        default: {
                            System.out.println("ERROR: Unknown Calculation Method");
                            dataError = true;
                            break;
                        }
                    }

                    if (dataError) {
                        break;
                    }
                }

                scanUserFile.close();

                howManyDataItems = CalcSD.getNumberOfDataItems();
                theAverage = CalcSD.calcAverage();
                theVariance =  CalcSD.calcVariance();
                theStDeviation =  CalcSD.calcStandardDeviation();

                if (howManyDataItems != INVALID) {
                    if (theAverage != INVALID) {
                        if (theVariance != INVALID) {
                            if (theStDeviation != INVALID) {

                                displayResults(howManyDataItems,theAverage,theVariance, theStDeviation);

                            }
                            else {
                                System.out.println("\nERROR Standard Deviation: NO VARIANCE Calculated");
                            }
                        }
                        else {
                            System.out.println("\nERROR Variance: NO AVERAGE Calculated");
                        }
                    }
                    else {
                        System.out.println("\nERROR Average: NO DATA Read from the file");
                    }
                }
                else {
                    System.out.println("\nERROR Data Read: NO DATA stored check file");
                }

            } // end of try

            catch (FileNotFoundException e) {
                System.err.format("File Not Found Exception: %s%n", e);
                e.printStackTrace();
            }

        }
        else {
            System.out.println("\n\n\tGood Bye");
        }
    } // end of main method
} // end of class


