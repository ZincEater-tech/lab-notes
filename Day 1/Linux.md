Linux/Terminal Cheat Sheet

cd /tmp = moves to temporary folder
pwd = shows where I am
ls = lists what's inside the folder
mkdir labpractice = made me a practice folder
cd labpractice = moves me into the practice folder
echo "login failed for user admin" > test.log = created me one line within the file (> = creates the file and replaces what's in it)
echo "login okay for user Josh" >> test.log = created a second line within the file (>> = adds a line to the end)
cat test.log = shows the lines within the file
grep failed test.log = shows only the lines with "failed" in them
ls -l test.log = shows the permissions (-rw-r--r--, other users could read the file)
chmod 600 test.log = changes the permissions, ran ls -l test.log again (-rw-------, only I can read and write the file)
rm -r labpractice = deleted the practice folder 

Practiced simple commands within terminal, writing down and seeing the visible changes with each command. Have used terminal (basic) for some of the projects I have done individually.