package com.company;

//------------------------------------ // The use of static imports is something that should be used carefully.
// I have only ever used this technique for the project wide constants. //
//
import static com.company.ProjConstants.*;

public class StDeviation {

/* ---------*---------*---------*---------*---------*---------*---------*---------*
// Class Level Variables
// ---------*---------*---------*---------*---------*---------*---------*--------- */

    // As discussed in class we are using a class populated with Project Constants to
// ensure help manage the project data. This ensures that each class can access the
// constant values, but in the event that a change is needed/required that this will
// only need to be made in one location, or file.
//
// Notice: If I had not done the "static import com.company.ProjConstants.*;" then
//         the use of the constant would have been written as:
//
//         private int[] Data = new int[ProjConstants.MAXDATA];
//
    private int[] Data = new int[MAXDATA];

    // sets all values to invalid to allow some checking
    private int sdItems = INVALID;
    private double sdAve = INVALID;
    private double sdVar = INVALID;
    private double sdDev = INVALID;

// ******************************************************************************
// ******************************************************************************

    private int sdCalcMethod = INVALID_CALC_METHOD;
    private int sdMinRange = INVALID_RANGE;
    private int sdMaxRange = INVALID_RANGE;


// THE USER WILL BE ALLOWED TO SELECT DISCRETE VARIABLES, OR DISCRETE VARIABLES
// IN  A FREQUENCY TABLE. (see changes made to the ProjConstants class)
//
//  The calcMethod class variable will be used to control which method of calculation
//  is to be used.
//
//  THE calcMethod VARIABLE MUST BE SET USING THE "set" AND "get" METHODS
//
// THERE ARE SEVERAL METHODS THAT WILL HAVE TO CHANGE IN THIS CLASS TO ALLOW THE
// CALCULATION OF STANDARD DEVIATION USING THE FREQUENCY TABLE METHOD AS WELL.
// PLEASE CONSIDER THE CHANGES THAT YOU WILL HAVE TO MAKE TO:
//   - addNewDataItem (precondition -  calculation method is set)
//   - calcAverage    (precondition -  data added, & calculation method is set)
//   - calcVariance   (precondition -  average calculated, data added, & calculation method is set)


    public void setCalcMethod(int how2calculate) {

        // Sets the class variable
        //  switch/case statement checks if "how2calculate" is DISCRETE, FRQTABLE,
        // GROUPED, if "how2calculate" is not one of these then it must be set to INVALID

        switch (how2calculate) {
            case DISCRETE:
                sdCalcMethod = DISCRETE;
                break;
            case FRQTABLE:
                sdCalcMethod = FRQTABLE;
                break;
            case GROUPED:
                sdCalcMethod = GROUPED;
                break;
            default:
                sdCalcMethod = INVALID_CALC_METHOD;
                System.out.println("ERROR: Standard Deviation Calculation Method either UNIMPLEMENTED, or UNKNOWN");
                break;
        }

    }

    public int getCalcMethod() {

        return sdCalcMethod;
    }

    public void setMin(int userIn) {

        if (userIn < MINDATA){
            sdMinRange = INVALID_RANGE;
        }
        else {
            sdMinRange = userIn;
        }

    }

    public int getMin() {

        return sdMinRange;
    }

    public void setMax(int userIn) {
       if (userIn > MAXDATA){
            sdMaxRange = INVALID_RANGE;
        }
        else {
            sdMaxRange = userIn;
        }
    }

    public int getMax() {

        return sdMaxRange;
    }



// ******************************************************************************
// ******************************************************************************

    // --------------------------------------------------
// The following method (procedure) will take a new data item (a parameter)
// and add it into the 1 Dimensional Array of data values to be used later.
//
//      Pre-Conditions:
//          - none
//
    public void addNewDataItem(int dataItem) {



        if ((sdItems == INVALID)) {
            sdItems = 0;
        }

            switch (getCalcMethod()) {

                case DISCRETE: {

                    Data[sdItems] = dataItem;
                    sdItems++;
                    break;
                }

                case FRQTABLE: {

                    if ((getMin() != INVALID_RANGE) && (getMax() != INVALID_RANGE)) {

                        if ((dataItem < getMin()) || (dataItem > getMax())) {
                            System.out.printf("ERROR: RANGE VIOLATION - Data Value ( %5.0f ), User Values: Minimum ( %5.0f ), Maximum ( %5.0f )",
                                    (double)   dataItem, (double) getMin(), (double) getMax());
                        } else if ((dataItem < MINDATA) || (dataItem > MAXDATA)) {

                            System.out.printf("ERROR: RANGE VIOLATION - Data Value ( %5.0 ), System Values: DATAMIN ( %5.0f ), DATAMAX ( %5.0f )",
                                    (double)  dataItem, (double) MINDATA, (double) MAXDATA);

                        } else {

                            Data[dataItem] = Data[dataItem] + 1;
                            sdItems++;

                        }
                    } else {
                        System.out.printf("ERROR: RANGE VIOLATION - Range values not set");
                    }
                    break;
                }

                case GROUPED: {
                }

                default: {
                    sdItems = INVALID;
                    sdCalcMethod = INVALID_CALC_METHOD;
                    System.out.println("ERROR: Standard Deviation Calculation Method either UNIMPLEMENTED, or UNKNOWN");
                    break;
                }
            }

        }


