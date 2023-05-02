Download Link: https://assignmentchef.com/product/solved-cpl-homework-8
<br>
<strong>141OS </strong>is a very simple operating system that allows multiple users to save and print files.​

The 141OS manages multiple disks, multiple printers, and multiple users – all of which can be considered concurrent processes. The goal of the system is to exploit possible parallelism to keep the devices (disks and printers) as busy as possible.

This will be a two week assignment where the following is needed:

<ul>

 <li>The first part is to define the Java classes for the <strong>backend</strong>​ ​</li>

 <li>The second part is to write the <strong>GUI portion</strong>​ using JavaFX or Swing.​</li>

</ul>




We will run all your code through <a href="https://theory.stanford.edu/~aiken/moss/">MOS</a>​          <a href="https://theory.stanford.edu/~aiken/moss/">S</a> <u>​ </u>which will detect any sharing of code (either between classmates or by copying any code found on the Internet).  <strong>Anyone whose code shows up as a copy</strong>​  <strong> will (at least) get a 0 for this entire homework assignment. </strong>




For this homework, we would like everyone to have the following project structure before you zip:




src/ # Has all your .java files inputs/    # Has your USER* input files for the program resources/ # Has your GUI resources (e.g. Disk.png, etc.)

Makefile # Has scripts that will compile builds and runs




For the Makefile, make sure it has something akin to this for building:




build: &lt;all your .java files&gt;

@echo —– creating a jar file with java —– jar cf 141OS.jar &lt;all your .java files&gt;




This creates a <a href="https://docs.oracle.com/javase/tutorial/deployment/jar/build.html">jar fil</a><u>​    </u><a href="https://docs.oracle.com/javase/tutorial/deployment/jar/build.html">e</a>, which will only be created if your files compile successfully. From here, we can​ run your program like this:




$ java -jar 141OS.jar &lt;args&gt;




Please conform to this so the autograder can work as intended.  Be sure to handle the command line arguments described below <u>LINK.</u><u>​         </u>




<strong>The autograder will be using openjdk version 8, so keep that in mind while building your project.</strong>

<strong>       </strong><strong> </strong>

<strong>] DESIGN SKETCH: PART ONE </strong>




You will be defining the classes that work as the backend of the 141OS. For this assignment, here are some important requirements regarding the operating system:




<ul>

 <li>Files can be stored on any disk and printed on any printer by any user.</li>

 <li>Only one line of a file may be stored in a sector, so a file will require one sector per line.</li>

 <li>Although Printers and Disks can be thought of as parallel processes, I suggest you not make them Threads. If you do, you must make them <a href="https://www.geeksforgeeks.org/daemon-thread-java/"><em>Daemon thread</em></a><u>​ </u><a href="https://www.geeksforgeeks.org/daemon-thread-java/"><em>s</em></a><a href="https://www.geeksforgeeks.org/daemon-thread-java/">​</a> ​by calling setDaemon or your program will never terminate.</li>

 <li>The classes with the name “Thread” in them, e.g., UserThread &amp; PrintJobThread, must be running on their own <a href="https://docs.oracle.com/javase/7/docs/api/java/lang/Thread.html">Threa</a>​ <a href="https://docs.oracle.com/javase/7/docs/api/java/lang/Thread.html">d</a>.​</li>

 <li>Use the <a href="https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuffer.html">StringBuffe</a>​ <a href="https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuffer.html">r</a>, which is implemented as an array of char. StringBuffer is the​ standard unit stored on disks, sent to printers <em>one line at a time</em>​  ​, and handled by users <em>one line at a time</em>​. We are using StringBuffers because Strings are not modifiable. Note also that StringBuffer.equals must do a deep equality if you plan to do comparisons.</li>

 <li>You can only have one user writing to a disk at a time or the files will be jumbled. You can have any number of Threads reading at a time – even when someone is writing to the disk. This is not normally the case in a real OS; I had to simplify the problem here.</li>

</ul>




<strong>NOTE: </strong>If you use a ​ <a href="https://docs.oracle.com/javase/7/docs/api/java/io/BufferedWriter.html">BufferedWrite</a><u>​ </u><a href="https://docs.oracle.com/javase/7/docs/api/java/io/BufferedWriter.html">r</a><a href="https://docs.oracle.com/javase/7/docs/api/java/io/BufferedWriter.html">,</a><u>​</u> be sure you flush the buffer after each write.




Your program <strong>must</strong>​ be runnable from command line and take command line arguments to​      define the number of users, disks, and printers. You should expect to follow this format:




$ java -jar 141OS.jar -#ofUsers &lt;Users&gt; -#ofDisks -#ofPrinters




An example input would be




<h2>$ java -jar 141OS.jar -3 USER1 USER2 USER3 -2 -3</h2>




This specifies that 3 users, 2 Disks, and 3 Printers are defined, with User1, User2, and User3 being files that mimic user input. Store instances of the appropriate objects in three separate arrays: Users, Printers, and Disks. Refer to <a href="https://docs.oracle.com/javase/tutorial/essential/environment/cmdLineArgs.html">thi</a>​  <a href="https://docs.oracle.com/javase/tutorial/essential/environment/cmdLineArgs.html">s</a> for how to pass arguments to your program.​




A sample of the input (USER*) and output (PRINTER*) is in the hw8 directory located in my Homework 8 directory <a href="https://www.ics.uci.edu/~klefstad/public/141/hw8/all.tar">her</a><u>​      </u><a href="https://www.ics.uci.edu/~klefstad/public/141/hw8/all.tar">e</a>. The PRINTER* files will be generated by your program, but the​   exact files that get sent to each printer may vary.

Below are the classes that you should define for the 141OS:

<table width="672">

 <tbody>

  <tr>

   <td width="672"><strong>Disk </strong></td>

  </tr>

  <tr>

   <td width="672">Each disk has a capacity specified to the constructor. The constructor must allocate all theStringBuffers (one per sector) when the Disk is created and must not allocate any after that. You can only read or write one line at a time, so to store a file, you must issue a write for each input line. You may implement it as an array of StringBuffers. Reads and writes each take <strong>200</strong>​       <strong> milliseconds</strong>, so your read/write functions must ​      <a href="https://docs.oracle.com/javase/tutorial/essential/concurrency/sleep.html">slee</a>​    <a href="https://docs.oracle.com/javase/tutorial/essential/concurrency/sleep.html">p</a> for that amount of time​         before copying the data to/from any disk sector.</td>

  </tr>

  <tr>

   <td width="672">class Disk { static final int NUM_SECTORS = 1024;StringBuffer sectors[] = new StringBuffer[NUM_SECTORS]; void write(int sector, StringBuffer data);  // call sleep void read(int sector, StringBuffer data);   // call sleep }</td>

  </tr>

 </tbody>

</table>




<table width="672">

 <tbody>

  <tr>

   <td width="672"><strong>Printer </strong></td>

  </tr>

  <tr>

   <td width="672">Each printer will write data to a file named PRINTERi where i is the index of this printer (starting at 1).  A printer can only handle one line of text at a time. It will take ​<strong>2750 milliseconds</strong>​ to print one line; the thread needs to ​<a href="https://docs.oracle.com/javase/tutorial/essential/concurrency/sleep.html">sleep</a>​ to simulate this delay before the printing job finishes.</td>

  </tr>

  <tr>

   <td width="672">class Printer { void print(StringBuffer data);  // call sleep}</td>

  </tr>

 </tbody>

</table>




<table width="672">

 <tbody>

  <tr>

   <td width="672"><strong>UserThread </strong></td>

  </tr>

  <tr>

   <td width="672">Each user will read from a file named USERi where i is the index of this user.  Each USERi file will contain a series of the following three commands:.save X.end.print XAll lines between a ​.save​ and a ​.end ​is part of the data in file ​X​. Each line in the file takes up a sector in the disk. UserThread will handle saving files to disk, but it will create a new PrintJobThread to handle each print request. Each UserThread may have only ​<strong>one local StringBuffer</strong>​ to hold the current line of text. If you read a line from the input file, you must immediately copy that String into this single StringBuffer owned by the UserThread. The</td>

  </tr>

 </tbody>

</table>

UserThread interprets the command in the StringBuffer or saves the StringBuffer to a disk as appropriate for the line of input.




<table width="672">

 <tbody>

  <tr>

   <td width="672"><strong>FileInfo </strong></td>

  </tr>

  <tr>

   <td width="672">A FileInfo object will hold the disk number, length of the file (in sectors), and the index of the starting sector (i.e. which sector the first line of the file is in). This FileInfo will be used in the DirectoryManager to hold meta information about a file during saving as well as to inform a PrintJobThread when printing.</td>

  </tr>

  <tr>

   <td width="672">class FileInfo { int diskNumber; int startingSector; int fileLength;}</td>

  </tr>

 </tbody>

</table>




<table width="672">

 <tbody>

  <tr>

   <td width="672"><strong>DirectoryManager </strong></td>

  </tr>

  <tr>

   <td width="672">The DirectoryManager is a table that knows where files are stored on disk via mapping file names into disk sectors. You should use the pre-defined <a href="https://docs.oracle.com/javase/7/docs/api/java/util/Hashtable.html">Hashtabl</a>​   <a href="https://docs.oracle.com/javase/7/docs/api/java/util/Hashtable.html">e</a> <u>​ </u>to store this information (in the form of FileInfo), but it must be private to class DirectoryManager.</td>

  </tr>

  <tr>

   <td width="672">class DirectoryManager { private Hashtable&lt;String, FileInfo&gt; T =      new Hashtable&lt;String, FileInfo&gt;(); void enter(StringBuffer key, FileInfo file); FileInfo lookup(StringBuffer key);}</td>

  </tr>

 </tbody>

</table>




<table width="672">

 <tbody>

  <tr>

   <td width="672"><strong>DiskManager </strong></td>

  </tr>

  <tr>

   <td width="672">The DiskManager is derived from ResourceManager (given below) and also keeps track of the next free sector on each disk, which is useful for saving files. The DiskManager should contain the DirectoryManager for finding file sectors on Disk.</td>

  </tr>

 </tbody>

</table>




<table width="672">

 <tbody>

  <tr>

   <td width="672"><strong>PrinterManager </strong></td>

  </tr>

  <tr>

   <td width="672">The PrinterManager is derived from ResourceManager.</td>

  </tr>

 </tbody>

</table>







<table width="672">

 <tbody>

  <tr>

   <td width="672"><strong>PrintJobThread </strong></td>

  </tr>

  <tr>

   <td width="672">PrintJobThread handles printing an entire print request.  It must get a free printer (blocking if the are all busy) then read sectors from the disk and send to the printer – one at a time. A UserThread will create and start a new PrintJobThread for each print request.  Note if each PrintJob is not a new thread you will get very little parallelism in your OS.</td>

  </tr>

 </tbody>

</table>







<table width="672">

 <tbody>

  <tr>

   <td width="672"><strong>ResourceManager </strong></td>

  </tr>

  <tr>

   <td width="672">A ResourceManager is responsible for allocating a resource like a Disk or a Printer to a specific Thread. Note you will have <em>two instances</em>​       ​: one for allocating Disks and another for allocating Printers, but I suggest you use inheritance to specialize for Disks and Printers. The implementation will be something like the following (you may use mine, but be sure you understand what it does and how it works; do you know why these methods must be synchronized?):</td>

  </tr>

  <tr>

   <td width="672">class ResourceManager { boolean isFree[];ResourceManager(int numberOfItems) { isFree = new boolean[numberOfItems]; for (int i=0; i&lt;isFree.length; ++i) isFree[i] = true;} synchronized int request() { while (true) { for (int i = 0; i &lt; isFree.length; ++i) if ( isFree[i] ) { isFree[i] = false; return i;} this.wait(); // block until someone releases Resource} } synchronized void release( int index ) { isFree[index] = true; this.notify(); // let a blocked thread run }}</td>

  </tr>

 </tbody>

</table>




I suggest you start on the GUI this week too (described below) – at least do the layout and any controls (e.g. start), and include some way to control the speed. You may choose to do PrintJobThread and ResourceManager in the next homework, but make sure the rest of the OS works as intended.




The autograder will test that you correctly create the supplied number of printers and that you print at least something in to each one of them. We will not test load balancing until the submission for homework 9.

<strong> </strong>

<strong>Submit the following: </strong>

<ul>

 <li><strong> a zip of the electronic version of your OS on Gradescope</strong><strong>                    </strong></li>

</ul>

<h1>Homework 9</h1>

<strong>Due Wednesday, March 11th at 2 AM, HARD DEADLINE </strong>

<strong> </strong>

<strong>[100] DESIGN SKETCH: PART TWO </strong>

<strong> </strong>

Build by hand-coding (i.e., don’t use a program to generate the GUI program) a GUI that displays the status of the 141OS. I want to see the following whenever the state of your program changes:




<ul>

 <li>what each <strong>User</strong>​ is doing​</li>

 <li>what each <strong>Disk</strong>​ is doing​</li>

 <li>what each <strong>Printer</strong>​ is doing​</li>

</ul>




The details of the GUI are left up to you. You may use <a href="https://en.wikipedia.org/wiki/Swing_(Java)">Swin</a>​ <a href="https://en.wikipedia.org/wiki/Swing_(Java)">g</a><a href="https://en.wikipedia.org/wiki/Swing_(Java)">,</a><u>​</u> ​<a href="https://docs.oracle.com/javase/7/docs/api/java/awt/package-summary.html">AWT</a>, or ​ <a href="https://docs.oracle.com/javase/8/javafx/get-started-tutorial/jfx-overview.htm">JavaF</a>​ <a href="https://docs.oracle.com/javase/8/javafx/get-started-tutorial/jfx-overview.htm">X</a> to develop the​

GUI to your liking. <u>Add speed control so the user can speed up or slow down the simulation!</u>​




If you do something impressive, I want to see a personal demo. We will give extra credit of zero, one, two, or three points (in course score scale) for impressive implementations of 141OS – depending on how impressive it is beyond the basic GUI. This is almost the value of <u>one entire homework assignment</u>.  Extra credit demos can be in office hours 10th week or on<u>​         </u> Monday of final’s week between noon and 5pm.  No extra credit demos after that time.  I will hold for demo appointments in DBH 3074.




<sup> </sup><strong>IMPORTANT (for both HW8 and HW9): </strong>Handle the following command line arguments that​   allows operation of your program <em>without</em>​        ​ the GUI. This will be done as a flag to your command line input via “-ng”:




<h2>$ java -jar 141OS.jar -2 USER1 USER2 -3 -4 -ng</h2>




It is <strong>very</strong>​ important you remember this step for the autograder.​




<strong>IF YOU ATTEMPT TO RUN GRAPHICS ON A HEADLESS MACHINE LIKE THE ONES THE AUTOGRADER USES, YOUR PROGRAM WILL CRASH. </strong>

























For the visual GUI, you will be submitting a URL to a video on YouTube. Here are the following requirements for the video:

<ul>

 <li>Use the sample inputs given in the homework as the User inputs.</li>

 <li>You will need to make <strong>2</strong>​<strong> Disks</strong> and ​ <strong>3</strong>​<strong> Printers</strong> (you can assume it will only be these values)​</li>

 <li>You need to have your <strong>Name</strong>​ and your ​ <strong>UCINetID</strong>​     visible in the GUI itself, either before the​         simulation or during the simulation.</li>

 <li>Take a screen recording of your GUI in action with these inputs in mind. This simulation will take around <strong>3</strong>​<strong> minutes</strong> approximately with the normal speed. Keep it at normal speed, but​ your speed control should be visible.</li>

 <li>Upload that screen recording onto <strong>YouTube</strong>​ , and submit the URL of that link onto the​         Canvas submission. You only need that link for the Canvas submission.</li>

</ul>




We will be looking for these things:

○   Printers

○    Disks

○    Users

○   Speed Control

○   <em>Concurrency</em>​ – this is <strong>important.</strong>​