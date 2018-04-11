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
// IN  A FREQUENCY TABLE.
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


// ------------------------------------------------------------------------
//
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






// ------------------------------------------------------------
// The following method (function) will return the total number of data
// items currently stored

    //      Pre-Conditions:
//          - none
//
    public int getNumberOfDataItems() {
        return sdItems;
    }


//------------------------------------------------------------
// The following method (function) returns a double precision value which
// is the average of all of the data values
//
//      Pre-Conditions:
//          - at least one data has been added
//          -calculation method has been set
//
    public double calcAverage() {

        double total = 0;

        if (sdItems != INVALID) {
            // Add all data values together (recall sdItems is the number of data items in the
            // Data storage array
            //
            switch (getCalcMethod()) {
                // if the user selects the calculation method in the Frequency Method Form,
                // the average will be calculated by...
                //
                case DISCRETE:
                    for (int i = 0; i < sdItems; i++) {
                        total += Data[i];
                    }
                    sdAve = total / (double) sdItems;
                    break;


                // if the user selects the calculation method in the Frequency Method Form,
                // the average will be calculated by...
                case FRQTABLE:{
                    for (int i = sdMinRange; i <= sdMaxRange; i++) {
                        total += (Data[i]*i);
                        sdAve= total/ sdItems;
                    }
                }
                break;

                case GROUPED:{

                }
                break;


                case INVALID_CALC_METHOD:
                    sdAve=INVALID;
                    break;
                // case GROUPED:
                // this will be the third part of the program
                // break;

            }// end switch (calcMethod)
        }
        else {
            // Pre-Conditions have not been met
            sdAve = INVALID;
        }

        // returns the calculated average
        return sdAve;
    }

// --------------------------------------------------
// The following method (function) returns a double precision value which is the Variance of all
// of the data values. If there is no data, or if the average has not been calculated
// then it returns INVALID
//
//      Pre-Conditions:
//          - at least one data has been added
//          - the average must have been calculated
//          - calculation method is set

    public double calcVariance(){

        double total = 0;
        double difference = 0;
        double diffSquared = 0;

        // Checks that data entry, and average have been done
        //
        if ((sdItems != INVALID) || (sdAve != INVALID)) {

            switch (getCalcMethod()) {

                case DISCRETE: {
                    for (int i = 0; i < sdItems; i++) {
                        total += Math.pow(Data[i] - sdAve,2);
                        sdVar= total / (double) sdItems;
                    }
                    break;
                }

                case FRQTABLE: {


                    // if data item does not equal previous item{
                    // create new value;}

                    // if data item equals previous item{
                    //increase counter (frequency) of value by 1;}

                   // Data [sdItems]++;


                    // sum of (value-average) squared times the frequency
                    // divide by the sum of all frequencies


                    for (int i = sdMinRange; i <= sdMaxRange; i++) {
                        //
                        difference = (i- sdAve);
                        diffSquared = Math.pow(difference,2);
                        total += diffSquared * Data[i];

                    }

                    sdVar= total / (double) sdItems;

                    break;
                }

                case GROUPED: {
                    break;
                }

                default: {
                    System.out.println("INVALID CALC METHOD, variance can not be obtained");
                    break;
                }


            }
        }     // Pre-Conditions have not been met sdVar = INVALID;
        else {
            sdVar = INVALID;
            System.out.println("INVALID VARIANCE");
        }

        return sdVar;
    }

// --------------------------------------------------
// The following method (function) returns a double precision value which is the Standard
// Deviation of all of the data values. If there is no data, no average, or if
// the variance has not been calculated then it returns INVALID
//
//      Pre-Conditions:
//          - at least one data has been added
//          - the average must have been calculated
//          - the variance must have been calculated
//

    public double calcStandardDeviation(){

        // Checks that data entry, average, and variance have been done
        if ((sdItems != INVALID) || (sdAve != INVALID) || (sdVar != INVALID)) {
            sdDev = Math.sqrt(sdVar);
        }
        else {
            // Pre-Conditions have not been met
            sdDev = INVALID;
        }

        return sdDev;
    }

        }// end class


