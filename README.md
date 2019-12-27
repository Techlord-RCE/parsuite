# parsuite

Simple parser framework. This was written super fast because I'm super impatient
when it comes to parsing stuff out of text files in various fomrats -- the whole
purpose of this little tool.

# A Note on Usage

Almost all irrelevant output, such as `[+] Alerting on some event`, is printed to 
`stderr`. When an output file isn't desired, you can always clean up the output by
sending `stderr` to the bitbucket and use `xclip` to catch the output from `stdout`,
.e.g `python3.7 ./parsuite.py nmap_top_port_dumper -t 100 2>/dev/null | xclip -sel clip`.

# Usage

## Getting General Help

Below is the current help menu detailing functionality and modules.

```
[+] Starting the parser
[+] Loading modules
usage: parsuite [-h]
                {base64_encoder,burp_info_extractor,burp_items_to_authmatrix,burp_to_authmatrix,crt_sh,csharp_hexarray_parser,enum4linux_dumper,hash_linker,ip_expander,ldap_dissection_xml_dumper,line_filter,moz_cookies_parser,nessus_api_host_dumper,nessus_output_dumper,nmap_smb_security_mode_dumper,nmap_ssl_name_dumper,nmap_top_port_dumper,nmap_xml_service_dumper,ntlm_hasher,ntlmv2_dumper,payload_inserter,prettyfi_json,rdp_sec_check_dumper,recon_ng_contact_dumper,socket_dum
per,string_randomizer,templatizer,urlcrazy_to_csv,xml_dumper}
                ...

Parse the planet.

positional arguments:
  {base64_encoder,burp_info_extractor,burp_items_to_authmatrix,burp_to_authmatrix,crt_sh,csharp_hexarray_parser,enum4linux_dumper,hash_linker,ip_expander,ldap_dissection_xml_dumper,line_filter,moz_cookies_parser,nessus_api_host_dumper,nessus_output_dumper,nmap_smb_security_mode_dumper,nmap_ssl_name_dumper,nmap_top_port_dumper,nmap_xml_service_dumper,ntlm_hasher,ntlmv2_dumper,payload_inserter,prettyfi_json,rdp_sec_check_dumper,recon_ng_contact_dumper,socket_dumper,string_ran
domizer,templatizer,urlcrazy_to_csv,xml_dumper}
                        Parser module selection.
    base64_encoder      Base64 encode a series of values or contents of files.
                        WARNING: files are slurped and encoded as a whole.
    burp_info_extractor
                        Input an XML file containing Burp items and dump each
                        transaction to a directory.
    burp_items_to_authmatrix
                        Parse an XML file containing Burp items and return a
                        JSON statefile for AuthMatrix. Each item element of
                        the input file must contain a "username" child element
                        and one or more "role" child elements. WARNING: THE
                        CHILD AND USERNAME ELEMENTS MUST BE ADDED TO EACH ITEM
                        MANUALLY!!!
    burp_to_authmatrix  Parse cookies from the results table of a Burp
                        Intruder attack and translate them to an Authmatrix
                        state file for those users. Warning: This tool assumes
                        that the username is in Payload1. Also make sure that
                        invalid records are removed from the table file,
                        otherwise they will be translated and added to the
                        JSON file.
    crt_sh              Query crt.sh and dump output to disk
    csharp_hexarray_parser
                        Parse C# shellcode from payload files generated by
                        Metsploit or Cobalt Strike. Useful when embedding
                        content in other formats.
    enum4linux_dumper   Dump groups and group memberships to disk, using the
                        filesystem as as basic database for simple searching
                        using grep and other mechanism.
    hash_linker         Map cleartext passwords recovered from password
                        cracking back to uncracked values.
    ip_expander         Expand a series of IPv4/6 ranges into addresses.
    ldap_dissection_xml_dumper
                        Dump LDAP objects from a file exported by Wireshark in
                        PDML format.
    line_filter         Remove lines found in bad files from lines found in
                        good files and write the resultant set of good lines
                        to an output file.
    moz_cookies_parser  Accept an Firefox cookie file (SQLite3) and dump each
                        record in CSV format. strLastAccessed and
                        strCreationTime are added to each record to help find
                        the freshest cookies. The final column contains the
                        constructed cookie.
    nessus_api_host_dumper
                        Extract affected hosts from the Nessus REST API.
                        Useful in situations when running a large scan or you
                        don't want to deal with exporting the .nessus file for
                        use with the `xml_dumper` module.
    nessus_output_dumper
                        Parse a Nessus file and dump the contents to disk by:
                        risk_factor > plugin_name
    nmap_smb_security_mode_dumper
                        Dump hosts to a file containing the security mode
                        discovered by smb-security-mode.
    nmap_ssl_name_dumper
                        Accept a XML file generated by Nmap and write SSL
                        certificate information to stdout
    nmap_top_port_dumper
                        Parse the Nmap services file and dump the most
                        commonly open ports.
    nmap_xml_service_dumper
                        Accept a XML file generated by Nmap and write the
                        output to a local directory structure, organized by
                        service, for easy browsing.
    ntlm_hasher         NTLM hash a value.
    ntlmv2_dumper       Parse files containing NTLMv2 hashes in the common
                        format produced by Responder and Impacket and dump
                        them to stdout. Messages printed that are not hashes
                        are dumped to stderr. Use the -du flag to disable
                        uniquing of username/domain/password combinations.
    payload_inserter    Define an insertion point (signature) within a
                        template file and replace the line with a payload from
                        a distinct file. Useful in situations where an
                        extremely long payload needs to be inserted, such as
                        when working with hex shellcode for stageless
                        payloads.
    prettyfi_json       Pretty print a JSON object to stdout.
    rdp_sec_check_dumper
                        Parse output from an rdp_sec_check scan and dump
                        output to individual files relative to issue, such as
                        "nla_not_enforced."
    recon_ng_contact_dumper
                        Parse an SQLite3 database generated by recon-ng and
                        dump the contacts out in simple string format
    socket_dumper       IPv4 ONLY! Accept a list of sockets and output three
                        files: unique list of IP addresses, unique list of
                        ports, unique list of fqdns
    string_randomizer   Accept a string as input and replace a template with
                        random values.
    templatizer         Accept a series of text templates and update their
                        values. It ingests a CSV file, making the values of
                        each field available to each template.
    urlcrazy_to_csv     Convert URLCrazy output to CSV
    xml_dumper          Dump hosts and open ports from multiple masscan, nmap,
                        or nessus files. A generalized abstraction layer is
                        used to produce objects that align with the Nmap XML
                        structure since it has the most comprehensive XSD
                        file.

optional arguments:
  -h, --help            show this help message and exit
```

## Getting Module Help

```
[+] Starting the parser
[+] Loading modules
usage: parsuite.py nessus_output_dumper [-h] --input-file INPUT_FILE
                                        --output-directory OUTPUT_DIRECTORY

optional arguments:
  -h, --help            show this help message and exit
  --input-file INPUT_FILE, -if INPUT_FILE
                        Input file to parse.
  --output-directory OUTPUT_DIRECTORY, -od OUTPUT_DIRECTORY
                        Output directory.
```

# Examples

## Parsing a Nessus File

The following command will parse a .nessus file and dump the contents to disk
organized by risk_factor > plugin_name. A file called msf_modules will be
included when a Metasploit module is available.

```
python3.6 parsuite.py nessus_output_dumper -if testfile.nessus -od test_output
[+] Output directory already exists!

Destroy and rebuild output? (destroy/no): destroy

[+] Checking /opt/git/parsuite/test_output/.tripfile before destroying...
[!] Destroying directory
[+] Creating new output directory: /opt/git/parsuite/test_output
Parsing: tcp:microsoft_windows_smb_nativelanmanager_remote_system_information_disclosure:445
Parsing: tcp:windows_netbios_smb_remote_host_information_disclosure:445
Parsing: tcp:dce_services_enumeration:49152
Parsing: tcp:dce_services_enumeration:52146
Parsing: tcp:dce_services_enumeration:49153
```

## Extracting IPs/Sockets/Ports from Nessus, NMap, and Masscan XML Files

The `xml_dumper` module accepts XML files from Nessus, NMap, and Masscan
and enables the user to query out interesting values in several formats:

- `address` - Unique IP addresses
- `port` - Unique ports
- `socket` - Standard socket format
- `uri` - Application layer URI, e.g. `http://192.168.1.1:80`

You can supply a port and service filter as well, which allows you to extract
proper records. For instance, you 

### Example

The URI format dumps services as a socket and leading application-layer protocol:

```
root@deskjet:recon~> parsuite xml_dumper -ifs full_aggressive.xml --format uri
[+] Starting the parser
[+] Loading modules
[+] Executing module: xml_dumper
[+] Module execution complete. Exiting.
cddbp-alt://192.168.1.92:8880
clearvisn://192.168.1.92:2052
gnunet://192.168.1.92:2086
http-proxy://192.168.1.92:8080
http://192.168.1.92:2053
http://192.168.1.92:2083
http://192.168.1.92:2087
http://192.168.1.92:2096
http://192.168.1.92:80
https-alt://192.168.1.92:8443
https://192.168.1.92:443
infowave://192.168.1.92:2082
nbx-ser://192.168.1.92:2095
cddbp-alt://192.168.1.92:8880
clearvisn://192.168.1.92:2052
gnunet://192.168.1.92:2086
http-proxy://192.168.1.92:8080
http://192.168.1.92:2053
http://192.168.1.92:2083
http://192.168.1.92:2087
http://192.168.1.92:2096
http://192.168.1.92:80
https-alt://192.168.1.92:8443
https://192.168.1.92:443
infowave://192.168.1.92:2082
nbx-ser://192.168.1.92:2095
```

And if you wanted to filter out any records containing the strings `http` or `proxy`,
you can use the `--sreg` and `--service-search` flags:

```
root@deskjet:recon~> parsuite xml_dumper -ifs full_aggressive.xml --format uri --sreg --service-search '(https?|proxy)'
[+] Starting the parser
[+] Loading modules
[+] Executing module: xml_dumper
[+] Module execution complete. Exiting.
http-proxy://192.168.1.92:8080
http://192.168.1.92:2053
http://192.168.1.92:2083
http://192.168.1.92:2087
http://192.168.1.92:2096
http://192.168.1.92:80
https-alt://192.168.1.92:8443
https://192.168.1.92:443
http-proxy://192.168.1.92:8080
http://192.168.1.92:2053
http://192.168.1.92:2083
http://192.168.1.92:2087
http://192.168.1.92:2096
http://192.168.1.92:80
https-alt://192.168.1.92:8443
https://192.168.1.92:443
```
