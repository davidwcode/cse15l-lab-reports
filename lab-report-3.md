# Week 5 Lab Report (10/31/22)

I'll be exploring the ```grep``` command in three command-line options:
```
--context=
--max-count=
-r
```

--- 

## Option 1
I'll be using ```--context``` in this one.

### Example 1:
```
$ grep --context=4 "Body mass index" technical/biomed/1468-6708-3-1.txt
        Data collection began in 1989, and follow-up is virtually
        complete for all surviving subjects [ 12 ] .
        
        
        Body mass index
        BMI was calculated as measured weight in kilograms
        divided by the square of measured height in meters. A
        report from the National Heart Lung and Blood Institute
        classifies normal weight (without reference to age) as a
--
--
        None declared
      
      
        Abbreviations
        BMI Body mass index
        CESD Center for Epidemiologic Studies Depression
        Scale
        CHS Cardiovascular Health Study
        EVGFP Is your health excellent, very good, good, fair or
```
In this example, the command prints out the instance of the searched text, "Body mass index" and also 4 lines before and after(determined from the number after each```--after-context```) after each instance of the text. This is useful in case you want to have a little more information about a specific topic like BMI from the file.

### Example 2
```
$ find technical > find-results.txt
$ grep --context=3 "Disaster" find-results.txt 
technical/government/Media/FY_04_Budget_Outlook.txt
technical/government/Media/help_rent-to-own_tenants.txt
technical/government/Media/Texas_Lawyer.txt
technical/government/Media/Disaster_center.txt
technical/government/Media/Kiosks_for_court_forms.txt
technical/government/Media/Higher_Registration_Fees.txt
technical/government/Media/Fire_Victims_Sue.txt
```
Here, I'm searching through a text file that has the names of every directory and text file (created with ```find```). The second command shows the file matching the keyword "Disaster" and the 3 preceding files and 3 following files. This can be useful for seeing the general content in the directory that the matching file was in.

### Example 3
```
$ grep --context=5 "33" technical/biomed/1471-213X-1-1.txt
        Gad1 mRNA translation or protein
        stability can be regulated in mature neurons by the level
        of GABA [ 30, 31]. During embryogenesis,
        post-transcriptional regulation occurs by alternative
        splicing during embryonic development in rats and mice [
        32, 33]. This alternate embryonic transcript inserts a stop
        codon into the 
        Gad1 mRNA and can produce the
        truncated proteins, GAD25 and GAD44, from its 5'; and 3'
        ends respectively. The studies reported here used a probe
        that will detect the adult 
```
In this example, I'm searching the file ```1471-213X-1-1.txt``` for "33," and I am getting the 5 preceding and following lines from the text file. This is useful for searching up how a specific reference is being used in a research paper, like this one, and what conclusions are being drawn from it.

---

## Option 2
These three examples will be using ```--max-count```.

### Example 1
```
$ grep --max-count=1 --context=2  "BMI" technical/biomed/1468-6708-3-1.txt
        Six large controlled population-based studies of
        non-smoking older adults have investigated the association
        between body mass index (BMI) and mortality, controlling
```
In this example (built off of the last option as well), although BMI appears multiple times in the file, the command only returns 1 instance because of the ```--max-count``` property. This can be useful if you only want to find the first instance of something in a passage to get a basic understanding of something.

### Example 2
```
$ find technical > find-results.txt
$ grep --max-count=3 "Law" find-results.txt
technical/government/Media/Law_Award_from_College.txt
technical/government/Media/Law_Schools.txt
technical/government/Media/Philly_Lawyers.txt
```
In this example, I'm searching the file created with find (just like Example 2 of the last option) that contains all the files and directories, and the command prints out three (from ```--max-count=3```) file names that match my search. This is useful in case you don't need every single match from ```grep```, a practical example of which would be finding three required sources for an essay.

### Example 3
```
$ grep --max-count=10 "aircraft" technical/911report/chapter-11.txt
                to Attack US Aircraft [with antiaircraft missiles]" (June 1998),"Strains Surface
                imagine that aircraft could be used as weapons. Indeed, since al Qaeda and other
                hijackings or the use of aircraft as weapons. It focused mainly on the danger of
                placing bombs onto aircraft-the approach of the Manila air plot. The Gore Commission
            Threat reports also mentioned the possibility of using an aircraft filled with
                explosives-laden aircraft into a U.S. city. This report, circulated in September
            Clarke had been concerned about the danger posed by aircraft since at least the 1996
                scramble aircraft from Langley Air Force Base, but they would need to go to the
                controls, and flown the aircraft into the sea. After the 1999-2000 millennium
            One prescient pre-9/11 analysis of an aircraft plot was written by a Justice
```
The ```--max-count``` option in this command returns the first lines instances that match the string "aircraft" in the file. This can be useful for getting a general gist of how a term is used without being overwhelmed by too much terminal output to read through.

---

## Option 3
Time to see what ```-r``` can do.

### Example 1
```
$ grep -r "Clinical trials" technical
technical/plos/pmed.0020007.txt:          controlling for this possibility [8]. Clinical trials are needed to test the hypotheses
technical/plos/journal.pbio.0020053.txt:        homes, and daycare centers. Fischetti says, “Clinical trials would tell us how often we had
technical/biomed/1471-2253-2-4.txt:        Clinical trials in acute pain normally last four to six
technical/biomed/ar309.txt:          P = 0.34). Clinical trials with
technical/biomed/cc103.txt:          Clinical trials of the effectiveness of diuretics or
technical/biomed/cc2167.txt:        excluded from participation. Clinical trials of treatment
technical/biomed/1475-2867-3-2.txt:        Clinical trials using the rapamycin analog CCI-779 show
technical/biomed/1472-6963-3-14.txt:            prediction of what will happen. Clinical trials
technical/biomed/cvm-2-6-278.txt:        Clinical trials
technical/biomed/1475-2867-2-10.txt:        reduced. Clinical trials are needed to test the use of this
technical/biomed/1471-2202-2-3.txt:        drug abusing women. Clinical trials are now necessary to
technical/biomed/1468-6708-3-1.txt:        decreased mortality. Clinical trials powered to detect
technical/biomed/1468-6708-3-1.txt:          YHL, but not YOL. Clinical trials of weight modification
technical/biomed/1468-6708-3-1.txt:          found for underweight older adults. Clinical trials whose
```
As can be seen here, ```-r``` allows grep to take in a directory and read all the files inside of it, which can be useful for searching for a specific term in a folder rather than an individual file. Here, it searches files in the biomed folder and returns all lines with "Clinical trials."

### Example 2
```
$ grep -r "Darwin" technical
technical/plos/journal.pbio.0020347.txt:        described by Charles Darwin (1859).
technical/plos/journal.pbio.0020347.txt:        Not all genetic variation is created equal. When Darwin first introduced the concept of
technical/plos/journal.pbio.0020347.txt:        evolution (Darwin 1859), he challenged the prevailing view that species were fixed entities
technical/plos/journal.pbio.0020346.txt:        on the traditional comparative approach, which was always the strength of Darwinian
technical/plos/journal.pbio.0020046.txt:        answers to possible questions and criticisms to avoid stuttering. Charles Darwin also
technical/plos/journal.pbio.0020046.txt:        stuttered; interestingly, his grandfather Erasmus Darwin suffered from the same condition,
technical/plos/journal.pbio.0020302.txt:        turn to be consumed by predators. Darwinian evolution would result in many of the same
technical/plos/journal.pbio.0020311.txt:        out by Charles Darwin and his son Francis in 1880. The Darwins were able to demonstrate
technical/plos/journal.pbio.0020071.txt:        are many ideologically motivated books opposing natural selection and Darwinism. To
technical/plos/journal.pbio.0020439.txt:        location within the head (Hsieh 2003). Charles Darwin was right when he wrote that people
technical/plos/journal.pbio.0020439.txt:        extra sense” (F. Darwin 1905). Today's biologists increasingly recognize that appropriate
technical/biomed/1471-2105-3-2.txt:        In the 1830's, Charles Darwin's investigation of the
technical/biomed/1471-2105-3-2.txt:        In the 1970's, Woese and Fox revisited Darwinian
```
In this example, ```-r``` allows ```grep``` to search through technical and read all the files. It returns all the lines with "Darwin" from all directories. This is useful because it shows that ```-r``` can recursively search through many layers of directories.

### Example 3
```
$ grep -r "Robert Woolard" technical
technical/government/Alcohol_Problems/Session2-PDF.txt:Robert Woolard, MD
technical/government/Alcohol_Problems/Session3-PDF.txt:Robert Woolard related that many ED patients they approached to
technical/government/Alcohol_Problems/DraftRecom-PDF.txt:Robert Woolard supported the recommendation and noted that most
technical/government/Alcohol_Problems/Session4-PDF.txt:Robert Woolard favored continuing intervention research in EDs.
```
In this example, the command once again searched recursively through technical and its subdirectories, government and Alcohol_Problems to find all mentions of Robert Woolard and print them in the terminal. This could be useful for finding all the work that an author has done in a big research folder.