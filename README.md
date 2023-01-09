# OMCI Wireshark Dissector
 * Install plugin dissects packets for ONT Management and Control Interface (OMCI) protocol (ITU Rec. G984.4).
   * Download [omci.lua](https://wiki.wireshark.org/uploads/__moin_import__/attachments/Contrib/omci.lua) and [BinDecHex.lua](https://wiki.wireshark.org/uploads/__moin_import__/attachments/Contrib/BinDecHex.lua)
   * Copy `omci.lua` and `BinDecHex.lua` to Wireshark project directory. (for example: on Windows C:\Program Files\Wireshark)
   * Add the following line at the end of this init.lua file: `dofile(DATA_DIR.."omci.lua")`
   
   Reference:<br>
      https://wiki.wireshark.org/Contrib<br>
      https://code.google.com/archive/p/omci-wireshark-dissector/wikis/WiresharkInstallation.wiki<br>
 * Regular Text Dumps
   * Text File Format
     * one packet per line and end of character ‘\n’<br>
     * Separate fields with whitespace
     * Supported fields
       1. **dir**: the direction the packet was sent over the wire; `<` for inbound and `>` for outbound.
       2. **time**: timestamp for the packet, format is “%H:%M:%S.%f” (Decimal Value)<br>
                    %H: `Hour`, %M: `Minutes`, %S: `Seconds`, %f: `Nanoseconds`
       3.	**data**: Actual captured frame/packet data (Hexadecimal Value)

     Example:<br>
       ![Example for text format](https://github.com/ChangHsingLee/omciWiresharkDissector/blob/main/picture/textFMT.png) 
   * Import text file
       1. Start Wireshark and click menu “File>Import from Hex dump…” to open dialog box<br>
        ![hexdump dialogbox](https://github.com/ChangHsingLee/omciWiresharkDissector/blob/main/picture/hexdump_dialogbox.png)
       2. To click “Regular Expression tab”
       3. To setup below import parameters
          * Determine which input file has to be imported, input it to text box of “file:”<br>
            ![textbox of file](https://github.com/ChangHsingLee/omciWiresharkDissector/blob/main/picture/textbox_of_file.png)
          *	Packet format regular expression, input below regular expression to text box.<br>
            `^(?<dir>[<>])\s(?<time>\d+:\d\d:\d\d.\d+)\s(?<data>[0-9a-fA-F]+)$`
	    
            ![textbox of regular expression](https://github.com/ChangHsingLee/omciWiresharkDissector/blob/main/picture/textbox_of_regex.png)
          * Data encoding(`Plain hex`) and Direction indication(`<` & `>`)<br>
            ![encoding and direction](https://github.com/ChangHsingLee/omciWiresharkDissector/blob/main/picture/encoding_direction.png)
          * Timestamp Format (`%H:%M:%S.%f`)<br>
            ![textbox of timestamp](https://github.com/ChangHsingLee/omciWiresharkDissector/blob/main/picture/timestamp.png)
          * Encapsulation type<br>
            Select “`Ethernet`” for encapsulation type and input “`88b5`” for Ethernet type<br>
            ![Ethernet Encapsulation Type](https://github.com/ChangHsingLee/omciWiresharkDissector/blob/main/picture/eth_encap_type.png)
       4.	Press “Import” button to start analysis your captured packets
       
       Reference:<br>
	     https://www.wireshark.org/docs/wsug_html_chunked/ChIOImportSection.html<br>
	     https://www.wireshark.org/docs/man-pages/text2pcap.html

