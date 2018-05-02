# ATA Beamformer Helper Scripts

These scripts are only useful at the Allen Telescope Array to control the beamformer. These scripts should only be run as atasys@sonata. Hence, these are for internal use only, no one else will get any use out of them.

These scripts start up the beamformer and calibrate. Once this script is finished the beamformer will be locked on to one or more targets and outputting beamformer packets over ethernet. Up to 3 beams, 2 pols each. So actually 6 individual beams.

The main script is bf_setup.rb. It does it all.

./bf_setup.rb <delay cal target> <phase target name> <BF1 freq MHz> <BF2/3 freq MHz> <beam1 target name> <beam2 target name> <beam3 target name> <logfile name tag>

 Example:
  ./bf_setup.rb casa 3c48 1680.0 1680.0 moon moon moon testlog1

You need to modify userinfo.rb to specify:

 * beamformers to use
 * ant pols to use
 * beam data output address and port

Trick - for the <phase target name> you can specify "best" (without quotes) and the script will pick out the best calibtaor to use.

## Other scripts

### best_cal.rb

This is a script that tells you the best phase calibrator to use for a given frquency.

  syntax: ./best_cal.rb: <freq in MHz>

  Example:

    > best_cal.rb 3000
      search for best cal @ 3000.0MHz among the following:
      	3c123
      	3c380
      	3c273
      	3c84
      	3c48
      	3c286
      3c123, flux 30.558 at 3000 MHz, UP - sets 21:18:27

  Contains this method used by bf_setup.rb:

    getBestCal(freq_mhz) - determines the best phase calibrator to use at a frequency, 
                           also returns the flux (in Jy) at that frequency.

### calinfo.rb

This is a script with methods used by bf_setup.rb. Gets the flux and set or rise time.

  syntax: #{$PROGRAM_NAME} <calibrator name> <freq in MHz>

  Example: 

    > ./calinfo.rb 3c84 3000
      3c84, flux 23.534 at 3000 MHz, UP - sets 20:48:45

  Contains several methods used by bf_setup.rb

    isUp(calName) - Determines is a calibrator is up, and also reports the rise or set time.

    getCalFlux(calname, freq_mhz) - Retries the flux of a calibrator at a certain frequency.

### shift_ephem.rb

  Reads an ATA ephemeris file and shifts the azimuth and elecation positions by a contant.

  syntax: shift_ephem <input file> <output file> <shift az deg> <shift el deg>



## Other programs


  offsetobs.rb - This is in development. Stay tuned. This script will allow you to perform 
                 observations with the beamformer, gridding around a target. More to come.
