# OMCI Wireshark Dissector
 * Install plugin dissects packets for ONT Management and Control Interface (OMCI) protocol (ITU Rec. G984.4).
   * Download [omci.lua](https://wiki.wireshark.org/uploads/__moin_import__/attachments/Contrib/omci.lua) and [BinDecHex.lua](https://wiki.wireshark.org/uploads/__moin_import__/attachments/Contrib/BinDecHex.lua)
   * Copy `omci.lua` and `BinDecHex.lua` to Wireshark project directory. (for example: on Windows C:\Program Files\Wireshark)
   * Add the following line at the end of this <ins>*init.lua*</ins> file: `dofile(DATA_DIR.."omci.lua")`
   
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
        3. **data**: Actual captured frame/packet data (Hexadecimal Value)
           * data format for Ethernet Type = `0x88b5` (Local Experimental EtherType 1)<br>
              data[0-47]: **OMCI frame**<br>
              Example:<br>
              ![Example for text format](https://github.com/ChangHsingLee/omciWiresharkDissector/blob/main/picture/textFMT.png)
           * data format for Ethernet Type = `0x88b7` (OUI Extended EtherType)<br>
              data[0-2]:  **ITU-T OUI**=0x0019A7 (ITU-T OUI Ethertype format)<br>
              data[3-4]:  **ITU-T Subtype**=0x0002 (OMCI per G.986)<br>
              data[5-6]:  **Packet Lenth**=0x0030 (OMCI packet length = 48 bytes)<br>
              data[7-54]: **OMCI frame**<br>
              Example:<br>
              ![Example for text format 88b7](https://github.com/ChangHsingLee/omciWiresharkDissector/blob/main/picture/textFMT_88b7.png)
	      Reference:<br>
	      https://www.itu.int/en/ITU-T/studygroups/2017-2020/15/Documents/IEEE-assigned_OUI-23-04-2021.docx<br>
              https://www.itu.int/rec/dologin_pub.asp?lang=e&id=T-REC-G.986-201001-I!!PDF-E&type=items or<br>
              https://www.itu.int/rec/dologin_pub.asp?lang=e&id=T-REC-G.9806-202202-I!Cor1!PDF-E&type=items<br>
              https://www.ieee802.org/1/files/public/docs2020/maint-seaman-protocol-ids-in-std-802-0520-v01.pdf<br>
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
            Select "`Ethernet`" for encapsulation type and input "`88b5`" or "`88b7`" for Ethernet type<br>
            ![Ethernet Encapsulation Type](https://github.com/ChangHsingLee/omciWiresharkDissector/blob/main/picture/eth_encap_type.png)
       4.	Press “Import” button to start analysis your captured packets
       
       Reference:<br>
	     https://www.wireshark.org/docs/wsug_html_chunked/ChIOImportSection.html<br>
	     https://www.wireshark.org/docs/man-pages/text2pcap.html

