<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#to-start">To start</a></li>
    <li><a href="#prepare">Prepare to submit jobs</a></li>
    <li><a href="#submit-jobs">Submit jobs</a></li>
    <li><a href="#cheat-sheet">Cheat sheet</a></li>
    <li><a href="#citation">To cite OSG</a></li>
  </ol>
</details>


<!-- To start -->
## To start
ask David Zurek for an OSG account. Other questions related to the OSG can be directed to Sajesh Singh (ssingh@amnh.org), who is the IT person who knows everything about OSG. 

<!-- Prepare to submit jobs -->
## Prepare to submit jobs
1. Create a folder with all the following files (assuming you are running python): [fib.sub](https://github.com/lyx12311/osg_tutorial/blob/main/fib.sub), [wrapper.sh](https://github.com/lyx12311/osg_tutorial/blob/main/wrapper.sh), and [python_build.tgz](https://zenodo.org/record/7324844/files/python_build.tgz?download=1).
 
2. wrapper.sh is the shell scrip to run your code, which typically contains the following:
* `tar xzf python_build.tgz` to unzip the python folder
* `python_build/bin/python3 <your script>.py $1` to run the python script on $1, which $1 will be obtained from the fib.sub file.

3. fib.sub is the submission file, the main things to change are: 
* This part is typically at the end of the fib.sub file. It indicates the arguments to pass down into the shell script.
```
queue fib from <filename>
```
this will execute wrapper.sh on every node with each line in <filename> as argument. 
E.g., if <filename> includes 3 lines that are:
aaa
bbb
ccc
It will execute ./wrapper.sh aaa,  ./wrapper.sh bbb, ./wrapper.sh ccc, on different nodes. This is very good for repeating jobs, e.g., each line is a light curve file you want to process.  

* This part is typically at the end of the fib.sub file. It indicates the arguments to pass down into the shell script.
```
executable = wrapper.sh
arguments = $(fib)
transfer_input_files = ztf_download.py, python_build.tgz, hotstars.csv
```

`executable = wrapper.sh` is the the executable shell script. 
`arguments = $(fib)` is the argument that is passed to the shell script. $1 in the shell script in this case, and fib is typically taken from a text file. It will run.
The last line represents the files you want to transfer to the node, it will typically include all the files you need to run the code besides the wrapper.sh and fib.sub file.

*This part is typically in the middle of the file, it will indicate whether you want to transfer the output data to your home where you submit the script. 
```
should_transfer_files = YES
when_to_transfer_output = ON_EXIT
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>




<!-- Submit jobs -->
## Submit jobs
Note: make sure your file has less than 5000 lines as you can only submit 5000 jobs at once.
```
condor_submit fib.sub 
```




<p align="right">(<a href="#readme-top">back to top</a>)</p>



