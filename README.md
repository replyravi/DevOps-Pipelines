# **DevOps Engineer Assignment – TaskD**

## **How did you test your pipelines?**
Testing the Jenkins pipelines involved multiple steps:

### **1 Local Validation Before Running in Jenkins**
- **Checked Jenkinsfile syntax** using:
  ```sh
  jenkins-linter Jenkinsfile
- ** Verified required tools are installed:**
  ```sh
  python3 --version
  doxygen --version
  git --version
### **2 Ran Individual Pipeline Stages Locally**
- ** For PipelineB (Doxygen Documentation Generation):**
  ```sh
  doxygen -g Doxyfile
  sed -i 's|INPUT.*|INPUT = src|' Doxyfile
  sed -i 's|GENERATE_HTML.*|GENERATE_HTML = YES|' Doxyfile
  doxygen Doxyfile
  tar -czf doc.tar.gz html/
- **  For PipelineC (Parsing Doxygen Logs in Python):**
  ```sh
  python parser.py warnings.log warnings.csv
  cat warnings.csv
### **3  Running in Jenkins**
- ** Created separate Pipeline Jobs in Jenkins for PipelineB and PipelineC.**
- ** Verified pipeline execution via:**
  ```sh
  Console Output (Jenkins > Pipeline > Build # > Console Output).
  Artifacts (Jenkins > Pipeline > Build # > Artifacts).
## ** How did you test RepoC Python?**
### **1 Local Testing of parser.py**
- ** Created a sample warnings.log:**
  ```sh
  Warning: Line 45, src/main.c: Unused variable
  Warning: Line 23, src/module.cpp: Possible memory leak
- **  Ran parser.py on the sample file:**
  ```sh
  python parser.py warnings.log warnings.csv
- **  Checked warnings.csv:**
  ```sh
  Line,File,Message
  45,src/main.c,Unused variable
  23,src/module.cpp,Possible memory leak
  
### **2 Automated Unit Tests**
- **  Added unit tests using pytest:**
  ```sh
  import pytest
  from parser import parse_warnings

  def test_parse_warnings():
    input_log = "test_warnings.log"
    output_csv = "test_warnings.csv"

    with open(input_log, "w") as f:
        f.write("Warning: Line 10, src/test.c: Test warning\n")

    parse_warnings(input_log, output_csv)

    with open(output_csv, "r") as f:
        lines = f.readlines()

    assert len(lines) == 2  # Header + 1 warning
    assert "10,src/test.c,Test warning" in lines[1]

  if __name__ == "__main__":
    pytest.main()
  
- ** Ran the tests:**
  ```sh
  pytest test_parser.py
## ** RepoA-doc contains binaries**
## ** What is the advantage of using Git LFS?**
- **  Git LFS (Large File Storage) optimizes the handling of large binary files by:**
 - ** 1 Reducing Repository Size – Prevents bloating the repo with binaries.**
 - ** 2 Efficient Cloning – Only downloads required files instead of all historical versions.**
 - ** 3 Better Performance – Git operations like clone and fetch run faster.**


## ** How to Adjust the Repository to Support LFS?**
### **1 Install Git LFS**
- **  Run the following command:
     ```sh
     git lfs install
### **2 Track Large Files**
- ** Track binary files (e.g., .bin, .tar.gz, .zip):**
    ```sh
    git lfs track "*.bin"
    git lfs track "*.tar.gz"
    git lfs track "*.zip"
- ** This creates a .gitattributes file:
     ```sh
     *.bin filter=lfs diff=lfs merge=lfs -text
     *.tar.gz filter=lfs diff=lfs merge=lfs -text
     *.zip filter=lfs diff=lfs merge=lfs -text
### **3  Commit and Push Changes**
    ```sh
       git add .gitattributes
       git add file.bin
       git commit -m "Added Git LFS support for binaries"
       git push origin main

## ** Are there other (easier) alternatives?**
  ### **1  Use Artifact Storage – Store binaries in external services like:**
   - ** AWS S3
   - ** Google Cloud Storage
   - ** Nexus / JFrog Artifactory
  ### **2  Use .gitignore – Instead of tracking binaries, ignore them:**
  *.bin
*.tar.gz
*.zip

 ### **3  GitHub Releases – Attach binaries as release assets instead of tracking them in Git.**





  

