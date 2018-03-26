# FrequencyTable

package com.company;

//---------*---------*---------*---------*
// The use of static imports is something that should be used carefully.
// I have only ever used this technique for the project wide constants.
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

    private int sdMinRange = INVALID;
    private int sdMaxRange = INVALID;


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


    private int calcMethod = INVALID;

    public void setCalcMethod(int how2calculate) {

        // Sets the class variable
        //  switch/case statement checks if "how2calculate" is DISCRETE, FRQTABLE,
        // GROUPED, if "how2calculate" is not one of these then it must be set to INVALID

        switch (how2calculate) {
            case DISCRETE:
                setCalcMethod(DISCRETE);
                break;
            case FRQTABLE:
                setCalcMethod(FRQTABLE);
                break;
            case GROUPED:
                setCalcMethod(GROUPED);
                break;
            default:
                setCalcMethod(INVALID_CALC_METHOD);
                break;
        }

    }

    public int getCalcMethod() {
        return calcMethod;
    }

    public void setMin(int userMin) {




    }

    public int getMin() {


    }

    public void setMax(int userMax) {


    }

    public int getMax() {


    }

    // val=getnext(){
    //if ((val< min)|| (val > max))
    // Print error
    // else {
    // Data[val]= Data[val]+1


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

        // In this case we have to check if we are adding the first data item.
        // If sdItems = -1 then no data has been previously added so we set
        // the number of items to zero

        if ((sdItems == INVALID)) {
            sdItems = 0;
        }

        switch (getCalcMethod()){

            case DISCRETE: {
                Data[sdItems] = dataItem;
                sdItems++;
            }

            case FRQTABLE: {
                if ((getMin()!= INVALID_RANGE) && (getMax()!= INVALID_RANGE)) {

                    if ((dataItem < getMin())||(dataItem > getMax())){
                        System.out.printf("ERROR: RANGE VIOLATION- Data Value (5.0%), User Values : Minimum (5.0%f), Maximum (5.0%f)",
                                dataItem, (double) getMin(), (double) getMax());


                    }
                }
            }
        }



    }

    // --------------------------------------------------
    // The following method (function) will return the total number of data
    // items currently stored

    //      Pre-Conditions:
    //          - none
    //
    public int getNumberOfDataItems() {
        return sdItems;
    }

    // --------------------------------------------------
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

                case DISCRETE:
                    for (int i = 0; i < sdItems; i++) {
                        total += Data[i];
                    }
                    sdAve = total /  (double) sdItems;
                    break;
                // if the user selects the calculation method in the Frequency Method Form,
                // the average will be calculated by...
                case FRQTABLE:
                    // calculate the average of using the method for the frequency table
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
                        difference = (Data[i] - sdAve);
                        diffSquared = Math.pow(difference, 2);
                        total = total + diffSquared;
                    }
                    break;
                }

                case FRQTABLE: {
                    break;
                }

                case GROUPED: {
                    break;
                }

                case default: {
                    System.out.printf("INVALID CALC METHOD, variance can not be obtained");
                    break;
                }


            }
        }
        else {

            System.out.printf("INVALID ... ");
        }
        /*

            // Loop over all data and calculate variance
            //


                // The calculation above could have been done on a single line
                // i.e. total += Math.pow( (Data[i] - sdAve), 2)
                // but it is easier to understand if done on separate lines.

            }

            // to calculate the variance we need to divide by the number of data items,
            // the "(double)" below instructs the computer to treat the integer value
            // of "sdItems" as a real number
            //
            sdVar = total / (double) sdItems;

        }
//        else {
            // Pre-Conditions have not been met
            sdVar = INVALID;
        }

*/
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

} // end of class
