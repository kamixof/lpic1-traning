# lpic1-traning

Exercise 1: Write a program that takes a number from the input, compares it to 10, and
returns a message for each of the 3 modes (larger, equal, and smaller).

#!/bin/bash
while [[ -z ${INT} || ! ${INT} =~ ^-?[0-9]+$ ]]
do
    read -p "Please Enter a Number: " INT
done
if [ ${INT} -gt 10 ]
then
    echo "${INT} is greater than 10"
elif [ ${INT} -lt 10 ]
then
    echo "${INT} is less than 10"
else
    echo "${INT} is equal to 10"
fi

#######################################################################################################
Exercise 2: Write a program that takes 20 numbers from the input, compares them, and
states which number is the largest and smallest.


#!/bin/bash

largest=""
smallest=""
count=1

for ((i=1; i<=20; i++)); do
  num=""
  while [[ -z ${num} || ! ${num} =~ ^-?[0-9]+$ ]]; do
    read -p "Please Enter ${count}th_Number : " num
  done

  if [[ -z ${largest} || ${num} -gt ${largest} ]]; then
    largest=${num}
  fi
  if [[ -z ${smallest} || ${num} -lt ${smallest} ]]; then
    smallest=${num}
  fi

  ((count++))
done

echo "The smallest number entered is ${smallest} and the largest number entered is ${largest}"
#########################################################################################################
Exercise 3: Write a program that has the IP of a server and its User/Pass in front of the Script
name and if it is pingable, sends its /etc/passwd file to /home/user path of that server,
otherwise a message displayed that the server is not accessible.

#!/bin/bash
server_ip="$1"
username="$2"
password="$3"

ping -c 1 ${server_ip} > /dev/null 2>&1

if [ $? -eq 0 ]; then

        echo "server is reachable"

        sshpass -p "$password" scp -o StrictHostKeyChecking=no /etc/passwd "${username}@${server_ip}:/${username}/"
        if [ $? -eq 0 ]; then
                echo "Copied is SUCCESSFULLY"
        else
                echo "Copied is FAILED"
        fi
else
        echo " server is NOT REACHABLE "

fi
############################################################################################################
Exercise 4: Write a program that prints from 5 to 50 on the screen.



#!/bin/bash


for (( i=5; i<=50; i++ )); do
        echo "${i}"


done
##############################################################################################################

Exercise 5: Write a program that saves the first and third fields of the /etc/passwd file every
day in a file with the same date and does not hold it for more than two days.


#!/bin/bash


if [[ ! -r /etc/passwd ]]; then
  echo "Error: Cannot read /etc/passwd" >&2
  exit 1
fi

output_dir="/passwd_BK"
mkdir -p "${output_dir}" || { echo "Error: Cannot create directory $output_dir" >&2; exit 1; }

output_file="$output_dir/passwd_$(date +%Y-%m-%d)"

awk -F':' '{print $1 " | " $3}' /etc/passwd > "$output_file" 2>/dev/null

if [[ ! -f "$output_file" ]]; then
  echo "Error: Failed to create output file $output_file" >&2
  exit 1
fi

find "$output_dir" -name "passwd_*.txt" -mtime +2 -delete 2>/dev/null

if [[ $? -ne 0 ]]; then
  echo "Warning: Could not delete old files" >&2
fi

echo "Successfully saved to $output_file and cleaned up old files"
exit 0

############################################################################################################

Exercise 6: Write a program that take a backup from home directory of your user after each
time user logged out.


############################################################################################################
Exercise 7: Write a program that reads, pings one by one from within a file containing the list
of destination IPs, and saves the result in a log file on the same day with the hostname of
that machine.


#!/bin/bash




while read -r line; do


        ping -c 1 -W 3 ${line} >> file2.txt
        if [ $? -eq 0 ]; then
                echo "Server with ip ${line} is REACHABLE"
        else
                echo "Server with ip ${line} is NOT REACHABLE!!!!"
        fi
done < file1.txt

###############################################################################################################



