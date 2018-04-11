package com.company;

//------------------------------------ // The use of static imports is something that should be used carefully.
// I have only ever used this technique for the project wide constants. //
//
import static com.company.ProjConstants.*;

public class StDeviation {

/* ---------*---------*---------*---------*---------*---------*---------*---------*
// Class Level Variables
// ---------*---------*---------*---------*---------*---------*---------*--------- */

    //The Project Constants class is used to help manage the project data.
//  This ensures that each class can access the constant values, but in the event that
//a change is needed/required that this will only need to be made in one location, or file.
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

    // when called upon this method will return the calculation method the user has chosen
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
//          - calculation method is set
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

                        System.out.printf("ERROR: RANGE VIOLATION - Data Value ( %5.0 ), System Values: DATAMIN ( %5.0f ),DATAMAX ( %5.0f )",
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
// items currently stored (int value)

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
//          -at least one data has been added
//          -calculation method has been set
//
    public double calcAverage() {

        double total = 0;

        if (sdItems != INVALID) {

            switch (getCalcMethod()) {

                // If the calculation method is DISCRETE - Discrete values
                case DISCRETE:
                    // the (double) total represents sum of all data values (Data[i]) together
                    // The average (sdAve) is set to the total divided by variable sdItems
                    // (recall sdItems is the number of data items in the Data storage array)

                    for (int i = 0; i < sdItems; i++) {
                        total += Data[i];
                    }
                    sdAve = total / (double) sdItems;
                    break;


                // If the calculation method is FRQTABLE - Discrete values in a frequency table

                case FRQTABLE:{
                    // (double) total represents the sum of all of the data values (variable i) divided by their frequency (Data[i])
                    // divides total by sdItems (the sum of all frequencies in the array/ the number of data items in array)

                    for (int i = sdMinRange; i <= sdMaxRange; i++) {
                        total += (Data[i]*i);

                        // The average (sdAve) is set to be the quotient of the total and sdItems
                        sdAve= total/ sdItems;
                    }
                }
                break;

                case GROUPED:{

                }
                break;

                // If the Calc-method is invalid, the average is also set to invalid
                case INVALID_CALC_METHOD:
                    sdAve=INVALID;
                    break;


            }// end  switch (getCalcMethod())
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
// of the data values. If there is no data, the calculation method is not set, or the average has not been calculated
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

                // If the calculation method is FRQTABLE- Discrete Values in a frequency table
                case FRQTABLE: {

                    // the variable difference represents the sum of the data values subtracted by the mean.
                    // variable diffSquared calculates square of the difference
                    // (double) total is product of sum of the diffSquared multiplied by
                    // the frequency for each unique data value

                    for (int i = sdMinRange; i <= sdMaxRange; i++) {
                        //
                        difference = (i- sdAve);
                        diffSquared = Math.pow(difference,2);
                        total += diffSquared * Data[i];

                    }
                    // the variation (sdAve) is the quotient of the total divided by the sum of all frequencies
                    sdVar= total / (double) sdItems;

                    break;
                }

                case GROUPED: {
                    break;
                }

                //if the calc-method is invalid, sets the variation to invalid
                default: {
                    sdVar = INVALID;
                    System.out.println("INVALID CALC METHOD, variance can not be obtained");
                    break;
                }


            }
        }     // Pre-Conditions have not been met sdVar = INVALID; print message to user
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


