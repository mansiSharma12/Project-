#!/bin/bash

# Predefined target files
target_files=(c_basefile.txt cpp_basefile.txt cs_basefile.txt)

while read -r filename; do
  # Extract extension
  extension="${filename##*.}"

  # Find matching target file
  target_file=""
  for file in "${target_files[@]}"; do
    if [[ "${file%.*}" == "$extension" ]]; then
      target_file="$file"
      break
    fi
  done

  # Check if target file found and file exists
  if [[ -n "$target_file" && -f "$filename" ]]; then
    echo "$filename" >> "$target_file"
  fi
done < basefile.txt

echo "Finished processing files and transferring content."




Incorporating exercise into the working hours of a job is becoming increasingly vital in the modern workplace. Firstly, regular exercise has a profound impact on physical health, helping employees maintain a healthy weight, reduce the risk of chronic diseases, and improve overall fitness. When employees are healthier, they are likely to take fewer sick days, resulting in increased productivity and reduced healthcare costs for both employers and employees. Moreover, exercise can boost energy levels and enhance mental clarity, which can lead to improved concentration and problem-solving skills. This increased cognitive performance can be particularly advantageous for tasks that require focus and creativity in the workplace.

Secondly, integrating exercise during working hours can be an effective way to reduce stress and promote mental well-being. The workplace is often a source of stress for many individuals, and stress can negatively impact productivity, job satisfaction, and overall mental health. Regular exercise, even in short breaks during the workday, can release endorphins, which are natural stress relievers. It helps employees manage stress levels, reduce anxiety, and maintain a positive attitude, ultimately contributing to a healthier and more conducive work environment.

Additionally, incorporating exercise at the workplace fosters a sense of community and teamwork among employees. Group workouts or physical activities can encourage social interaction, improve team dynamics, and enhance employee morale. This sense of camaraderie can lead to better collaboration, problem-solving, and a more enjoyable work atmosphere, thus increasing overall job satisfaction and reducing turnover rates. In summary, integrating exercise into the working hours of a job is not just about physical fitness; it's about improving employee health, mental well-being, and the overall work environment, resulting in a win-win situation for both employers and their workforce.
