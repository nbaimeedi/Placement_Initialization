#### Steps to generate HPWL value for baseline i.e., RePLACE

clone the OpenROAD flow scripts repository
we can find rtls with different technology nodes under design.

change into the flow directory

Run the design using make file using the following command:

DESIGN_CONFIG=./designs/tech_node/rtl/config.mk make

it will start the entire ASIC flow. Since we are only interested in Placement results we can modify the make file to stop after placement.
However, the commenting  out lines after placement in Makefile would throw errors as the final part of Makefile has code to move all files into correct directories.
We have two ways to proceed -
  1. Modify the Makefile to stop after placement
  2. Run the entire process and get required information from the files that are saved after placement.

We are taking the second approach. To save time we kill the flow after we see that the tool has finished the placement phase. 
This way, we do not need to modify Makefile while avoiding unneccesary space and time on post-placement tasks.

Since we are using HPWL value after global placement step as metric, we can find the file under logs/3_3_place_gp.log
We repeat the process for every data point.

Note - 

DreamPLACE needs def and gate level netlist files before global placement. The current openroad flow gives the gate level netlist only, not the def file.
To generate the def file before global placement step, we add the following line to flow/scripts/floorplan.tcl

write_def $::env(RESULTS_DIR)/floorplan.def

Then the gate level netlist along with this def file is given as input to DreamPLACE code global placement. This way we can obtain the HPWL values
from DreamPLACE script. This process is repeated for every data point.
