# NuancePowerCreate
Use Nuance's (now Kofax) PowerPDF program to traverse a root directory through all children with a given filetype, and combine/convert/make searchable, etc.

This article explains how to perform batch conversion of PDF, TIFF, and other image file formats into PDF, PDF Searchable, and TIFF files via a command line interface, using Nuance's latest document imaging software — Power PDF Advanced.
Power PDF is the newest product from the Document Imaging division of Nuance Communications. It is available in two editions — Power PDF Standard and Power PDF Advanced. Nuance offers a free 30-day trial of the Advanced edition, available for download here:

http://www.nuance.com/for-business/imaging-solutions/document-conversion/power-pdf-converter/free-trial/index.htm



For whatever reason, Nuance does not offer a free trial of the Standard edition. Everything in this article about Version 1.0 is based on my experience with the free trial of the Advanced edition running in W7 Pro 64-bit; everything about Version 1.1 is based on the licensed Power PDF Advanced, also on W7 Pro 64-bit.



Disclaimer before going further: I have no affiliation with Nuance and no financial interest in it whatsoever. I am simply a happy user/customer of several of its document imaging products, including OmniPage, PaperPort, and PDF Converter (the latter is being replaced by Power PDF).



Update on 22-Feb-2015: The initial submission of this article was about version 1.0 of Power PDF Advanced. Nuance recently released version 1.1, but since EE members may still have 1.0, I decided not to delete the information about 1.0, but instead added a section at the bottom of the article about 1.1.

End of update.



Update on 18-Jun-2015: Although undocumented in the Help text, I just discovered that the input files may be other image file types, including GIF, JPG, and PNG (the Help text documents only PDF and TIF).

End of update.



Update on 23-Aug-2017: Since my previous article update about version 1.1, Nuance released 1.2 in February 2016, 2.0 in June 2016, and 2.1 in May 2017. Some key points about these versions:



(1) The list of OCR languages is identical in all five releases (1.0, 1.1, 1.2, 2.0, 2.1).



(2) The Help text in both 1.2 and 2.0 is identical to the Help text in 1.1, which is shown below in the 1.1 section of the article.



(3) The Help text in 2.1 has three differences from 1.1/1.2/2.0, namely:

• the -T output type parameter has an additional choice: TXT (thanks to member Amine BEN DHIAB for reporting this in the comments section below on 16-Jun-2017)

• the -V version parameter for PDF output has five additional choices: X1A, X3, X4, X4P, PDFE

• there's a new -Y option for PDF output, which means convert to gray

In terms of big differences between the Advanced edition of 1.x and 2.x, here's a Nuance article discussing it:
What's new in Power PDF Advanced 2.0?

End of update.



Update on 9-Aug-2018: Since my previous article update about versions 1.2, 2.0, and 2.1, Nuance released Version 3.0 on 5-Jun-2018. As with my prior article updates, I did not delete information about the earlier versions, instead adding a section at the bottom of the article about 3.0.

End of update.



The Graphical User Interface (GUI) of Power PDF Advanced (PPA) utilizes the Ribbon interface, made popular by Microsoft in Office 2007 and continued in all Office releases since then. The Home section of PPA's Ribbon looks like this:



PPA Home Ribbon

The Advanced Processing section of the Ribbon has a Batch Converter function:

 

PPA Batch Converter GUI

This is an excellent feature that provides for batch conversion of PDF and TIFF files into PDF, Searchable PDF (via built-in OCR), TIFF, and Multipage TIFF. But as I was putting the product through its paces during the 30-day trial, I wondered if there is a Command Line Interface (CLI) for batch conversion. Nothing is documented in the offline or online help about a CLI for the batch converter, and Nuance hasn't (yet) released a user guide/manual for the product. So I took a look at the installation folder "c:\Program Files (x86)\Nuance\Power PDF\" to see if anything jumped out with respect to a CLI for batch conversion, and sure enough, found "BatchConverter.exe" and "BatchConverter.com" executables. At first I thought they were there to support the [Batch>Batch Controls>Batch Converter] item shown on the Ribbon (in the screenshot above), but then I decided to run "BatchConverter" in a Command Prompt in the off-chance that it provides a CLI. Jackpot! I gave it the standard "/?" parameter for help and, lo and behold, this came back (turns out that "-?" also works):



(See important note after this Help text — spoiler alert: batchconverter.exe is wrong — batchconverter.com is correct.)



Usage:



batchconverter.exe -I<inputfile> -O<outputfile> -T<output type>
[-A<PDF/A format>] [-R] [-Q] [-P<page number>]
[-S<colorspace>] [-D<resolution>] [-Cm<cmpmono>]
[-Cg<cmpgray>] [-Cc<cmpcolor>] [-L<lang1, lang2, lang3>]
[-J<char>] [-K] [-Ao] [-As] [-?] [-F<logfile>]



-I  input file full path. * can be used for filename (*.pdf, *.tif)

 -O  output file or folder full path.

 -T  output type: TIF|PDF|PDFS

 -A  PDF/A format: 1A|1B|2A|2B|2U|3A|3B|3U. Default:Non-PDF/A

 -R  recursive processing, subfolders under the input will be processed too. The input folder structure will be created under the output folder.

 -Q  quiet processing, no password dialogs will be showed thus password protected pdf files will not be converted successfully.

 -P  process single page

 -?  Shows usage info and the list of languages + 3letter language identifiers.

 Tiff output specific parameters

 -S  colorspace: auto|mono|gray|rgb. Default: auto.

 -D  resolution: auto|72|96|150|200|300|600|1200|2400. Default: auto.

 -Cm  image compression for monochrome images: none|g3|g4|lzw. Default: g4.

 -Cg  image compression for grayscale images: none|LZW|JpegMin|JpegLow|JpegMed|JpegHigh|JpegMax. Default: LZW.

 -Cc  image compression for color images: none|LZW|JpegMin|JpegLow|JpegMed|JpegHigh|JpegMax. Default: LZW.

 Searchable PDF output specific parameters

 -L  Language(s) for OCR. Comma delimited list of 3letter language identifiers (-Leng,ger,fre,nor). If not presented, auto is used.

 -J  Reject char. Default: ~.

 -K  If presented, conversion keeps original images.

 -Ao  If presented, pages are oriented and straightened automatically.



That's not the end of the help output — it continues with a list of 123 languages for OCR support. To avoid scrolling for many screens in this article, I removed the list of languages from the help output above and placed it in the attached text file called "Power-PDF-Advanced-OCR-Languages.txt".



Important Note:   BatchConverter.exe is the Batch Converter GUI, while BatchConverter.com is the Batch Converter CLI. If you are in a Command Prompt in the folder containing both files and enter the command BatchConverter (that is, without the .com or .exe extension) the CLI will run and work as expected. However, if you enter the command BatchConverter.exe, it will not run the CLI. It won't even recognize the /? or -? parameter. The syntax in the help output shown in the "Usage:" section above is incorrect! It says the command is batchconverter.exe, but it should say batchconverter.com.



I then ran numerous tests and it worked flawlessly! Here's an example that converts an image-only PDF to a searchable PDF (-TPDFS), keeping the images in the output file (-K), and using English for the OCR (-Leng):



batchconverter -Id:\testin\testimage.pdf -Od:\testout\testsearch.pdf -TPDFS -K -Leng



All the parameters provide extremely useful features, but in my opinion, these stand out:

 

  Creation of a PDF Searchable file (the -TPDFS option), with an additional option (-K) that keeps the images.
 

  Processing subfolders to an unlimited depth and automatically replicating the input folder structure under the output folder, called recursive processing (the -R option).
 

  A wide variety of compression methods for TIFF files, with separate methods for monochrome (-Cm), grayscale (-Cg), and color (-Cc) images.
 

  An amazing choice of languages for OCR (the -L option).
As mentioned above, I was not able to find any documentation on the CLI, except for the output from the -? parameter. Everything I know about the Batch Converter CLI is from the command itself — and experimentation with it. So if anyone reading this article learns anything interesting about it, please post it here for all to see.



In conclusion, PPA's CLI provides capabilities that cannot be achieved in the GUI. It may be called from batch files, scripts, and custom programs. I'm looking forward to using it and I hope this article documenting it helps some other EE members. If you do find this article to be helpful, please endorse it by clicking the thumbs-up icon at the end of the article. This lets me know what is valuable for EE members and provides direction for future articles. Thanks very much! Regards, Joe



Update for Version 1.1

This is the Help text for Version 1.1 (see important note after this output) :



Usage:



batchconverter.com -I<inputfile> -O<outputfile> -T<output type>
[-R] [-Q] [-P<page number>] [-S<colorspace>]
[-D<resolution>] [-Cm<cmpmono>] [-Cg<cmpgray>]
[-Cc<cmpcolor>] [-V<version>] [-W] [-G] [-E<sequence name>]
[-L<lang1, lang2, lang3>] [-J<char>] [-K] [-Ao] [-As] [-?] [-F<logfile>]



-I  input file full path. * can be used for filename (*.pdf, *.tif)

 -O  output file or folder full path.

 -T  output type: TIF|PDF|PDFS

 -R  recursive processing, subfolders under the input will be processed too. The input folder structure will be created under the output folder.

 -Q  quiet processing, no password dialogs will be showed thus password prote cted pdf files will not be converted successfully.

 -P  process single page

 -?  Shows usage info and the list of languages + 3letter language identifiers.

 Tiff output specific parameters

 -S  colorspace: auto|mono|gray|rgb. Default: auto.

 -D  resolution: auto|72|96|150|200|300|600|1200|2400. Default: auto.

 -Cm  image compression for monochrome images: none|g3|g4|lzw. Default: g4.

 -Cg  image compression for grayscale images: none|LZW|JpegMin|JpegLow|JpegMed|JpegHigh|JpegMax. Default: LZW.

 -Cc  image compression for color images: none|LZW|JpegMin|JpegLow|JpegMed|JpegHigh|JpegMax. Default: LZW.

 PDF output specific parameters

 -V  Version: 1A|1B|2A|2B|2U|3A|3B|3U|1.3|1.4|1.5|1.6|1.7|auto

 -W  Optimize for web viewing

 -G  Remove tags

 -E  Execute sequence

 Searchable PDF output specific parameters

 -L  Language(s) for OCR. Comma delimited list of 3letter language identifiers (-Leng,ger,fre,nor). If not presented, auto is used.

 -J  Reject char. Default: ~.

 -K  If presented, conversion keeps original images.

 -Ao  If presented, pages are oriented and straightened automatically.



Important Note:  Although not stated in the Help output above (which shows only PDF and TIF), I discovered through experimentation that the input file type may also be GIF, JPG, JPEG, PNG, and TIFF. In other words, the -I (Input) parameter should say this:



-I  input file full path. * can be used for filename (*.gif, *.jpg, *.jpeg *.pdf, *.png, *.tif, *.tiff)



I tried BMP, but that did not work. If readers discover other file types that work, please post a comment to let us know.



As with 1.0, the 1.1 Help output includes the long list of languages, which I omitted above. I compared the 1.0 and 1.1 language lists — they are identical.



Note that the syntax error documented in the 1.0 portion of the article has been fixed in 1.1, that is, the Usage output now shows the correct  batchconverter.com (rather than the incorrect  batchconverter.exe of 1.0).



One very important bug that is relevant to this article has been fixed in 1.1, namely, the -P parameter (to process a single page) in conjunction with an output type of PDF or PDFS. This did not work in 1.0 — it created an output PDF file with all the pages of the input file rather than just the one page specified (it worked if the output type was TIF). The -P parameter now works for all output types in 1.1.



The Release Notes for 1.1, including a list of new features, a list of known issues, and a list of bug fixes, are in a PDF file called Nuance Power PDF 1.1 Release Notes in the Files section of this wiki.



Update for Version 3.0

This is the Help text for Version 3.0:



Usage:

batchconverter.com -I<inputfile> -O<outputfile> -T<output type>
[-R] [-Q] [-P<page number>] [-S<colorspace>]
[-D<resolution>] [-Cm<cmpmono>] [-Cg<cmpgray>]
[-Cc<cmpcolor>] [-V<version>] [-W] [-G] [-E<sequence name>]
[-L<lang1, lang2, lang3>] [-J<char>] [-K] [-Ao] [-?] [-F<logfile>]

-I      input file full path. * can be used for filename (*.pdf, *.tif)

-O      output file or folder full path.

-T      output type: TIF|PDF|PDFS|TXT|MTIF

-R      recursive processing, subfolders under the input will be processed too. The input folder structure will be created under the output folder.

-Q      quiet processing, no password dialogs will be showed thus password protected pdf files will not be converted successfully.

-P      process single page

-?      Shows usage info and the list of languages + 3letter language identifiers.

Tiff output specific parameters

-S      colorspace: auto|mono|gray|rgb. Default: auto.

-D      resolution: auto|72|96|150|200|300|600|1200|2400. Default: auto.

-Cm     image compression for monochrome images: none|g3|g4|lzw. Default: g4.

-Cg     image compression for grayscale images: none|LZW|JpegMin|JpegLow|JpegMed|JpegHigh|JpegMax. Default: LZW.

-Cc     image compression for color images: none|LZW|JpegMin|JpegLow|JpegMed|JpegHigh|JpegMax. Default: LZW.

PDF output specific parameters

-V      Version: 1A|1B|2A|2B|2U|3A|3B|3U|1.3|1.4|1.5|1.6|1.7|2.0|X1A|X3|X4|X4P|PDFE|auto

-W      Optimize for web viewing

-G      Remove tags

-E      Execute sequence

-Y      Convert to gray

Searchable PDF output specific parameters

-L      Language(s) for OCR. Comma delimited list of 3letter language identifiers (-Leng,ger,fre,nor). If not presented, auto is used.

-J      Reject char. Default: ~.

-K      If presented, conversion keeps original images.

-Ao     If presented, pages are oriented and straightened automatically.



The list of OCR languages has not changed — it is identical to all previous 1.x and 2.x versions.





If you find this article to be helpful, please click the thumbs-up icon below. This lets me know what is valuable for EE members and provides direction for future articles. Thanks very much! Regards, Joe
