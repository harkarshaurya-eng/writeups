//Reading Endianess of Python

hacker@reverse-engineering~reading-endianness-python:~$ cd /challenge
hacker@reverse-engineering~reading-endianness-python:/challenge$ ls
DESCRIPTION.md  cimg
hacker@reverse-engineering~reading-endianness-python:/challenge$ flie cimg
bash: flie: command not found
hacker@reverse-engineering~reading-endianness-python:/challenge$ file cimg
cimg: setuid Python script, ASCII text executable
hacker@reverse-engineering~reading-endianness-python:/challenge$ cat cimg
#!/usr/bin/exec-suid -- /usr/bin/python3 -I

import os
import sys
from collections import namedtuple

Pixel = namedtuple("Pixel", ["ascii"])


def main():
    if len(sys.argv) >= 2:
        path = sys.argv[1]
        assert path.endswith(".cimg"), "ERROR: file has incorrect extension"
        file = open(path, "rb")
    else:
        file = sys.stdin.buffer

    header = file.read1(4)
    assert len(header) == 4, "ERROR: Failed to read header!"

    assert int.from_bytes(header[:4], "little") == 0x726E6F28, "ERROR: Invalid magic number!"

    with open("/flag", "r") as f:
        flag = f.read()
        print(flag)


if __name__ == "__main__":
    try:
        main()
    except AssertionError as e:
        print(e, file=sys.stderr)
        sys.exit(-1)
hacker@reverse-engineering~reading-endianness-python:/challenge$ pushd /tmp
/tmp /challenge
hacker@reverse-engineering~reading-endianness-python:/tmp$ nano solve.py
hacker@reverse-engineering~reading-endianness-python:/tmp$ cat solve.py 
with open("solve.cimg", "wb") as f:
    f.write(b"\x28\x6f\x6e\x72")
hacker@reverse-engineering~reading-endianness-python:/tmp$ python solve.py 
hacker@reverse-engineering~reading-endianness-python:/tmp$ ls
bin  hsperfdata_root  solve.cimg  solve.py  tmp.hYVUanCOAv
hacker@reverse-engineering~reading-endianness-python:/tmp$ popd
/challenge
hacker@reverse-engineering~reading-endianness-python:/challenge$ ./cimg /tmp/solve.cimg 
pwn.college{AOsj8BtX2RzN-k4QDDLcB2muVjy.01NwUjNxwCN2cDOxIzW}

hacker@reverse-engineering~reading-endianness-python:/challenge$ 
