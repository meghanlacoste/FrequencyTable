# FrequencyTable
package com.company;

public class ProjConstants {

        // ---------*---------*---------*---------*---------*
        // Integer Constants
        public static final int INVALID = -1;
        public static final int INVALID_CALC_METHOD = -11;
        public static final int INVALID_RANGE       = -21;


        // ******************************************************************************
        // THE FOLLOWING CONSTANTS HAVE BEEN ADDED TO ALLOW THE USER TO SELECT/SET THE
        // METHOD USED
        //   NOTE: This could have been done with an enumerated type but for
        //         simplicity it is being done here
        // ******************************************************************************

        public static final int DISCRETE = -111;
        public static final int FRQTABLE = -112;
        public static final int GROUPED  = -113;

        public static final int MAXDATA = 2000;
        public static final int MINDATA = 0;

        // PRIVATE //

        // ---------*---------*---------*---------*---------*---------*---------*---------*
        // The caller references the constants using ProjConstants.MAXDATA,
        // and so on. So the caller should be prevented from constructing objects of
        // this class.
        // --> By declaring this private constructor for the class we accomplish this. <--
        //
        private ProjConstants() {
                //this prevents even the native class from calling this constructor as well
                throw new AssertionError();
        }
}
