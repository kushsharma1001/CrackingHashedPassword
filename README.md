# CrackingHashedPassword
Simple way to crack a hash value of a password using Hashcat. Read more here: https://github.com/brannondorsey/naive-hashcat

Genreally, to crack it we need a GPU, but for now lets download docker and run a hashcat image in intel cpu.

1) docker run -t -i dizcza/docker-hashcat:intel-cpu /bin/bash
2) execute: cd hashcat-5.1.0/
3) wget https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt

Now, Let's pretend for a moment that we were able to compromise a target machine, and pull a password hash off of the box.
say hash value we found from some js on website: $5$Ebkn8jlK$i6SSPa0.u7Gd.0oJOT4T421N2OvsfXqAT1vCoYUOigB

Now that we have the password hash let's put it into a file named user_password_hash using this command:
echo """\$5\$Ebkn8jlK\$i6SSPa0.u7Gd.0oJOT4T421N2OvsfXqAT1vCoYUOigB""" > user_password_hash

remember its important to escape dollar sign in hashed password so that the bash command runs successfully.

Before we attempt to crack this hash we need to know what algorithm was used to produce the it. Hashcat happens to have an excellent list of example hashes that can be found here.

These are examples of various encoding algorithms and their associated Hash-Mode.

In this case as we go down the list you can see that the SHA256 (Unix) example is the closest match to the hash we want to crack. Hence, we will now use hashmode 7400:

4) execute: ./hashcat64.bin -m 7400 user_password_hash rockyou.txt    (here -m takes a hashmode from here: https://hashcat.net/wiki/doku.php?id=example_hashes which matches the hashes value best.)
