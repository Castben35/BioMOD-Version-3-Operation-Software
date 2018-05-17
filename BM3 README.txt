BioMOD 3.1 Software Tips: Steps for integrating sensors with BioMOD 3.7.5 Software (Labview)
by: Benjmain Punshon-Smith : ben35@umbc.edu
Center for Advanced Sensor Technology: Unviersity of Maryland, Baltimore County
Date: November 20th, 2017

BioMOD 3.1 is an automated device which integrates mulitple sensors to measure and perform protein liquid chromatography. Its communication
protocol relies on FTDI based communication for the syringe pumps, valves, uv sensors, and indirect pressure sensors.

LabVIEW 7.1 is used for the BioMOD Operation Software: "BIOMOD_3.7.5_SOFTWARE.vi" -> "BIOMOD_3.7.5_SOFTWARE.exe"

*Necessary naming convections for integrated Sensors: USE "FTPROG" software to rename string descriptor for devices.

1.) UV sensors Texas intruments: MSP430 based optical absorbance sensor: FTDI naming conventions
- can accomidate up to 5 UV absorbance sensors: whose FTDI String descriptors must be set from the list below:
	- "UV Sensor 0", "UV Sensor 1", ..., "UV Sensor 5" 
	
- In setup of MSP430-based UV sensor set device Serial Number from the list below:
	- "AB0", "AB2", "AB3", "AB4", "AB5"  (this is not FTDI serial number)
	
2.) Indirect Pressure Sensors: Openscale (Sparkfun) connected to load cell:
- can accomidate up to 4 pressure sensor boards, whose FTDI's string descriptors must be labelled as:
	- Loading pump: 	"FT231X USB UART1"
	- Wash 2 pump: 		"FT231X USB UART2"
	- Elution pump: 	"FT231X USB UART3"
	- Polishing pump	"FT231X USB UART4"
	
- Arduino Code for Openscale board is labelled: "IndirectPressure2.0", and should be uploaded to each pressure sensor board.
	- connect the Openscale board to comuputer with Ardunio
	- Program 'IndirectPressure2.0.ino' to device
	- open Arduino "Serial Monitor"
	- Type 'x' command in serial monitor to print openscale board settings
		- type command = 'q' >>>> Raw Readings (ENABLE)
		- type command = 't' >>>> SERIAL TRIGGER (ENABLE)
	- Type 'x' command to close and disconnect device
	
3.) Valve Driver Board: controls up to 4 valves ( 2 for sample colleciton, 2 for bioprocess control)
- Name FTDI string descriptor of vavle driver board as "vavle driver"
- make sure valve order is in line with that in the bioprocess diagram. Use "PUMP CONTROL": button in BioMOD software to check valves manually

4.) Pump Driver: 5 New Era pumps and chain-linked to a FTDI USB-to-Serial Converter.
- FTDI converter must have a string descriptor named "pump driver" ( set this in FTPROG)
	- Address New Era pumps as "1" through "5" (New Era manual describes method of doing this)
	- * USE Serial command : "ADR01" to set connected pump to adress "1", check adress by sending "01ADR" (response "01ADR01")
- Make sure to set syringe pump diameters to correct value for syringe used (use "PUMP CONTROL" in BioMOD software to current pump settings)
	- Once addressed, you can use the "PUMP CONTROL" subprogram within the BIOMOD Software (front page, 'Step #2' section) to change diameters as follows:
		- Open "Manual Control of pumps and valves subprogram by clicking the "PUMP CONTROL" button in the BIOMOD software
		- Set "Pump No." to the address of pump of interest
		- Under the dropdown button labelled "Syringe Volume" with value "mm DIA", choose from the syringe sizes listed
		- The default syringes used are as follows
			- Pump 1 - "10 mL "
			- Pump 2-5 - "60 mL"
		- *Using the New Era serial command (DIA) will aslo change the pump diameters: examples (command 01DIA14.43) (set pump 1 to BD 10mL syringe( 14.43 mm diameter))
		- The list of diameters for BD syringes are as follows
		- 1mL - 4.70 mm
		- 3mL - 8.58 mm
		- 10mL - 14.43 mm
		- 30 mL - 21.59 mm
		- 60 mL - 26.59 mm  (01DIA26.59)
		
		
Calibration of Sensors: Indirect Pressure Sensors, and UV sensors are calibrated with respect to a reference device : PendoTECH, and Thermo Scientific Ultimate 300 variable wavelength detector (at 280 nm)
- A linear calibration can be performed from the appropriate ranges:
	- UV sensor (BSA 2 500 ng/mL to 2mg/ML) : 20 mAU to 1.2 AU 
	- Pressure Sensor: 0.2 psi to 28 psi  (New Era pump Limit is 30 psi)

BIOMOD_Version_3.exe: Steps in order to setup DEFAULT configuration and automatic storage of run Data files.
 	- in order to tailor the program to your systems computer please edit the configuration files in the 'BioMOD 3.0/Configuration Files' folder
	- edit either file of 'Configure_BioMOD3_1A_DEFAULTMicro.txt' or 'Configure_BioMOD3_1A_DEFAULTMicro.txt' as such:
		- Edit the two path strings in the configuration folder to corespond to your computer heirarchy
		- example: 'C:\Users\Tristation2\Desktop\BioMOD 3.0\Run Data 2017' -> C:\<Your Path>\BioMOD 3.0\Run Data 2017

Necessary Drivers:
- Labview 7.1 RunTime Engine needs to be download in order to run the BIOMOD_Version_3.exe executable on a computer. Install this before operation of the program.
	- Labview runtime engine can be install from the link: http://www.ni.com/download/labview-run-time-engine-7.1/703/en/
- FTDI drivers must also be install ( though plugging in an FTDI device will trigger windows to automatically install these for you, other wise visit FTIDI's website at http://www.ftdichip.com/Drivers/D2XX.htm)
