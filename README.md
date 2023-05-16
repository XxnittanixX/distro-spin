# distro-spin
#!/bin/bash                                                                                                                                                                                               
                                                                                                                                                                                                              
    echo "Select a USB drive to boot from:"                                                                                                                                                                   
    selected_drive=$(lsblk | grep -E 'sd.|disk' | awk '{print $1, "(" $4 ")"}' | tr '\n' '\0' | xargs -0 -I {} sh -c 'echo {}| fzf' | awk '{print $1}')                                                       
                                                                                                                                                                                                              
    if [ -z "$selected_drive" ]; then                                                                                                                                                                         
      echo "No USB drive selected. Exiting."                                                                                                                                                                  
      exit 1                                                                                                                                                                                                  
    fi                                                                                                                                                                                                        
                                                                                                                                                                                                              
    echo "Selected drive: $selected_drive"                                                                                                                                                                    
                                                                                                                                                                                                              
    iso_path='/path/to/iso'                                                                                                                                                                                   
    if ! [ -f "$iso_path" ]; then                                                                                                                                                                             
      echo "ISO file not found. Exiting."                                                                                                                                                                     
      exit 1                                                                                                                                                                                                  
    fi                                                                                                                                                                                                        
                                                                                                                                                                                                              
    echo "Select a Linux distribution to install:"                                                                                                                                                            
    cd /tmp                                                                                                                                                                                                   
    printf "Archcraft\nUbuntu\nDebian\n" | fzf | while read -r distro; do                                                                                                                                     
      echo "Selected distro: $distro"                                                                                                                                                                         
      sudo dd bs=4M if="$iso_path" of="/dev/$selected_drive" status=progress oflag=sync                                                                                                                       
    done                                                                                                                                                                                                      
                                                                                                                                                                                                              
    echo "Done."                                                                                                                                                                                              
                                                                                                                                                                                                              
  This script first displays a list of USB drives that are available for booting using  lsblk , and prompts the user to select one. It then checks for the existence of the ISO file that you want to write to
  the USB drive, and prompts the user to select a Linux distribution (in this example,  Archcraft ,  Ubuntu , or  Debian ). Once a distribution is selected, the script uses  sudo  and  dd  to write the ISO 
  to the USB drive.                                                                                                                                                                                           
                                                                                                                                                                                                              
  Make sure to replace  /path/to/iso  with the path to your ISO file. Also, keep in mind that you'll want to make sure you're running this script as  root  or with  sudo .                                   

