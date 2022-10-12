#!/bin/bash

# Create a new directory for analysis.
mkdir CAFire
cd CAFire

# Download the dataset and give it a new name
curl  https://gis.data.cnra.ca.gov/datasets/CALFIRE-Forestry::recent-large-fire-perimeters-5000-acres.csv  > CAFire.csv

# Print out the range of years that occur in this file.
cut -d, -f2 CAFire.csv | sort | uniq

# Correct file: 2 weird numbers,“ 48088” and “6901”, come from the previous line. Because a line of data is speared into two lines, the second line produce weird number.
# Delete comma symbol in “Comment” column in OBJECTID ”42033”, “43183” and “41656” for further analysis.
nano CAFire.csv

# Print out the range of years to confirm the correction.
sed 1d CAFire.csv > CAFire2.csv
echo "The range of years is $(cut -f2 -d"," CAFire2.csv | sort -n | head -1 ) and $(cut -f2 -d"," CAFire2.csv | sort -n | tail -1)"
	
# Print out the number of fires in the database
echo "The number of Fires is $( wc -l  < CAFire2.csv )."

# Print out the number of fires that occur each year
echo "The number of fires each year:
Number Year
$(awk -F, '{print $2}' CAFire.csv | sort -n | uniq -c | tail -n +2)"

# Print out the name and year of largest fire
echo "The largest fire
GIS_ACRES Fire_Name Year
$(awk -v FS="," -v OFS="," '{print $13,$6,$2}' CAFire.csv | sort -nr | head -1)"

# Print out the total acreage burned in each year
cut -d, -f2 CAFire2.csv | sort | uniq > ArchYear.txt
ValYear=$(cat ArchYear.txt)
for YEAR in $ValYear
   do
      Acre=$(grep $YEAR CAFire.csv | awk -F',' '{sum+=$13;} END{print sum;}')
      echo "The total acreage burned in $YEAR was $Acre"
   done

