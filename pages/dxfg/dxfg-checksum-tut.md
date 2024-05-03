---
title: Checksum Verification of Documents
last_updated: May 1, 2024
summary: "Information about using a web service to verify documents issued by an authoritative body."
permalink: dxfg-checksum-tut.html
toc: true
mathjax: true
layout: page
---
## Introduction
Checksums are used to detect changes in digital records, perhaps caused during transmission, or storage, or by some other operation. Checksums are short blocks of data (e.g., strings), which are readily calculated on computer systems. A checksum algorithm produces different values for different inputs (documents). So, it is possible to establish the integrity of digital records by comparing the initial checksum of a document with a checksum evaluated later. 

Here we describe a checksum method of verifying digital documents issued by a calibration laboratory. The method allows automatic verification without exposing information contained in a document. It also allows the issuing laboratory to cancel documents at any time (e.g., when a correction to the original document is required). The method would be provided as a service on the internet, which is queried using a checksum to identify a document. 

A simple proof-of-concept in Python is given [below](#example-running-on-localhost).

## Benefits of checksums
Checksums can give confidence in the integrity of documents formally issued by a laboratory. 

If the checksum of every document issued by a laboratory is recorded, the checksums can help the laboratory to verify the authenticity of documents later, as required (e.g., for legal purposes). This is particularly pertinent in the case of documents that cannot be verified just by eye (where digital contents may differ but the appearance may not).

When a document is transmitted to a customer, the checksum can be provided, allowing the customer to verify that no errors occurred during transmission. 

### Cancellation and reissue of documents 
However, laboratories occasionally need to cancel a report and re-issue a corrected document. In these circumstances, customers need to remove existing copies of the original document and replace them with the corrected version. Confusion can easily arise, because the cancelled version of a digital document will still appear to be valid. 
 
## An online checksum registry
Providing an online register of valid checksums is a way that an issuing laboratory can improve their document management and help their customers. Customers can submit the checksum of any document to the register to automatically determine its current status. 

Verification using an online registry service does not expose the document itself. Only the checksum is transmitted to the server, which is completely anonymous. So, this approach does not raise concerns about privacy and confidentiality of information.


### Example running on localhost
The following Python script demonstrates the idea. The script runs locally. 

Save the following code in a file (e.g., `checksum.py`)
```py
from flask import Flask
from gevent.pywsgi import WSGIServer

port = 8080
app = Flask(__name__)

checksums = set()

@app.route('/verify/<checksum>')
def verify(checksum):
    return str( checksum in checksums )

if __name__ == '__main__':
    
    # These SHA-256 sums will be recognised
    checksums.update([
    '62cdb7020ff920e5aa642c3d4066950dd1f01f4d18ef927cfb523c4982f1b240',
    'd014d80bacc6b7e1ef4cc93fe45b31e74a107a296635f91cf2aebc8a586c4280',
    'b6e7ee0900d39c8a79b9cc1d512d93ebddb16c690e605f75851ea7e268fcbff5',
    '00e3e5af2b1eb28cc998fc187cd6c5b146e5d7d62c7dc960a8b2df2a2c3eb0b3',
    'f98ce315a9f91f82f39befe40f0278128a68db6b78856b3c283dcbe34811b7fd',
    '71d7ea22b4f72a3850f7a2c8c76d9f7ec53482e8a69494a5e35b5e13406c6803',
    'ff26de0543feccde8c7ca66311846f33c29df95b2476e337b047b21f25b5fd85',
    'daed81f29ce32d487d3dd2b4c54d2e1a39c45846083de129640c9a45f4c95f6b',
    'e35cb47c2b992ed2b3e16a535179db1c763ac4dc16b8477db97a2fe82eeef6db',
    '6b2d2a9ab37b4e18859e5e03d922f6cefded92a85d7b01d800eaa9c41864cc90'
    ])
    
    try:
        with WSGIServer(('0.0.0.0', port), application=app) as server:
            print(f'Server running at http://localhost:{port}')
            print('Press CTRL+C to quit')
            server.serve_forever()
    except KeyboardInterrupt:
        pass
```  

To run the server, first ensure that Python packages `flask` and `gevent` are installed (if necessary run `pip install flask gevent`). Then execute the script at the command line, e.g. (if the script was saved as `checksum.py`): 
```
> python checksum.py
```
 
{% include note.html content=" 
If running the python command raises and error similar to either <br>
<tt>&emsp; OSError: [Errno 98] Address already in use: ('0.0.0.0', 8080)​</tt><br>
or<br> 
<tt>&emsp; OSError: [WinError 10048] Only one usage of each socket address (protocol/network address/port) is normally permitted: ('0.0.0.0', 8080)</tt>​<br>
then change the value of the `port` parameter (e.g., port = 8000) and re-run the command."
%}

An internal collection of 10 checksums are included in this script. Open a web browser and go to `http://localhost:8080/verify/a-checksum-string`, where `a-checksum-string` is a string of characters. 

"True" will appear in the browser window if one of the 10 defined checksums is used (e.g., `http://localhost:8080/verify/62cdb7020ff920e5aa642c3d4066950dd1f01f4d18ef927cfb523c4982f1b240`). Otherwise, "False" will appear.

{% include note.html content="This example has no mechanism to add and remove checksums from the collection. Secure access to a database of checksums would be needed."
%}


### Calculating checksums 

There is a variety of checksum algorithms available. The MD5 checksum is often used to manage the integrity of digital documents that are transmitted on the internet. However, MD5 checksums have several vulnerabilities. More secure hashing algorithms like SHA-256 or SHA-3 are preferred for document verification and data integrity checks.

On Windows computers, the SHA-256 checksum can be calculated using the `CertUtil` command-line utility, as follows
```
> CertUtil -hashfile "path_to_your_file" SHA256
```
On UNIX computers the checksum can be calculated by
```
> shasum -a 256 "path_to_your_file"
```

### A Use Case

Suppose NMI-X issues a report to Customer-Y. The report is a PDF/A document that contains several embedded digital files. NMI-X sends the report to Customer-Y by email.

 * Customer-Y can verify the integrity of the report document at any time, by evaluating the checksum and using it to query the NMI-X online verification server. 
 * Indeed, Customer-Y can automate this, allowing the report to be checked, before data is extracted, whenever it is used.
 * Furthermore, checksums for the embedded files can be verified online. So, if Customer-Y has extracted the embedded files to a different location in their file system, the integrity of those files can be checked independently.
 
If NMI-X needs to re-issue the report and change any of the embedded files, the corresponding checksums are removed from the verification server database. 
 * Customer-Y can detect the report's status immediately and can also identify any embedded files in the original that remain valid.