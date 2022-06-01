

# Using a printer shared by another CUPS


To register a printer shared by another CUPS, you can simply use the command:

    lpadmin -p printer -E -v ipp://server/printers/printer

I got this from this page [http://www.cups.org/documentation.php/doc-1.4/sharing.html](http://www.cups.org/documentation.php/doc-1.4/sharing.html)
