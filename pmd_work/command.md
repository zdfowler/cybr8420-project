1. Download PMD https://pmd.github.io/
2. Extract to a spot on your computer
3. Add extracted path's bin to your computer's path
   just follow quickstart on the main page from above ^^
4. Make sure you have a downloaded copy of keeweb's source.

This is the command to run PMD, for javascript (ecmascript), and output summary HTML.  Change -t to other options like HTML, CSV, text, or others for varying output.  See pmd -? for more.

```
pmd.bat -d c:\path\to\keewebsourc\ -R ecmascript-basic -t summaryhtml
```

Or, pipe this to a file for saving.


```
pmd.bat -d c:\path\to\keewebsourc\ -R ecmascript-basic -t summaryhtml > summaryhtml.html
```

Sample summary file here: [SummaryHtml.html](https://github.com/zdfowler/cybr8420-project/blob/master/pmd_work/summaryhtml.html) but be sure to save as html.
