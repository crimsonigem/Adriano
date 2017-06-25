# Week 3 Write-Up

### Outreach and Presentations

From Friday through Sunday this week, the Harvard iGEM club (which is distinct from the Harvard iGEM team) is hosting a bio-hackathon at Harvard. Nevertheless, the team got involved in the process since we saw it as a great opportunity for community engagement. We gave a brief presentation about bio-ethics at the kick-off of the hackathon, and we were also available at a help desk Saturday morning.

Earlier in the week, we presented at NEGEM - a gathering of New England iGEM teams. MIT, Boston University, Worcester Polytechnic Institute, Tufts, UConn, and Northeastern were all present besides our Harvard team.

![NEGEMphoto](/figures/IMG_4737.jpg)
This is a photo of our team at the Center for Integrated Life Sciences and Engineering at Boston University following the NEGEM gathering.

The upshot is that our week was marked by more presentation and outreach preparation than actual science and planning, so I don't have too much technical material to include in this post. I sort of got into the spirit of the hackathon, though, and started brushing up on my Python a bit.

### String Manipulations in Python

 I wrote some very basic string manipulation functions in Python, and the hope is that I'll be able to add to these and use them at the beginning of next week to order our ultramers for MoClo DNA assembly. There's really nothing in the code that wouldn't be understandable after a short primer with Python. My data structure for sequences includes both the sequence (i.e. the order of a, t, c, and g) and a bool which indicates the directionality of the strand (5' to 3' or 3' to 5'). In the framework I've made, I treat DNA sequences as a linear construction; this is limiting because it then becomes difficult to represent a circular piece of DNA like a plasmid. Also, I am treating DNA as a single-stranded structure right now, though it is not difficult to change the dictionary so that I include double-stranded information. I even have hashed comments in my code - with too much detail perhaps - following from my only coding experience in CS50.

 def check(DNA):
     # check that DNA variable is a dict
     if type(DNA) is not dict:
         print("Not a valid DNA variable")
         return 1
     # check that DNA dictionary has two fields
     if len(DNA) != 2:
         print("Not a valid DNA variable")
         return 1
     # check that DNA sequence is valid
     seq = DNA['seq'].lower()
     for i in range(len(seq)):
         if seq[i] != "a" and seq[i] != "t" and seq[i] != "g" and seq[i] != "c":
             print("Not a valid DNA sequence")
             return 1
     # check that DNA direction is a bool
     if type(DNA['dir']) is not bool:
         print("Not a valid DNA direction")
         return 1

     return 0

 def DNAinput():

     # requests DNA sequence
     print("DNA sequence: ")
     seq = input()

     # ensures correct direction of DNA sequence input
     print("Direction 5' to 3'? (y/n)")
     dir_raw = input()

     # stores bool based on user's response
     # 5' to 3' direction is treated as "True"
     if dir_raw.lower() == "y":
         dir = True
     elif dir_raw.lower() == "n":
         dir = False
     else:
         print("Invalid input")
         return 1

     # makes the dictionary
     DNA = {'seq': seq, 'dir': dir}

     # sends the construct through check to make sure it is valid data
     if check(DNA) != 0:
         print("Invalid responses")
         return 1

     # returns the data structure
     return DNA

 def ligate(DNA_list):

     # ensures that input is a list
     if type(DNA_list) is not list:
         print("Not a list")
         return 1

     # notes direction of first strand, initiates new dictionary for ligated DNA
     dir = DNA_list[0]['dir']
     DNA_ligated = {'seq': "", 'dir': dir}

     # loops through DNA list
     for i in range(len(DNA_list)):
         if check(DNA_list[i]) != 0:
             print("Invalid list")
             return 1
         elif DNA_list[i]['dir'] != dir:
             print("DNA sequences with different directions")
             return 1

         DNA_ligated['seq'] += DNA_list[i]['seq']

     return DNA_ligated

 def flip(DNA):

     # ensures that a dictionary is given
     if type(DNA) is not dict:
         print("Not valid data structure")
         return 1

     # ensuress that DNA is a valid sequence
     if check(DNA) != 0:
         print("Not a valid DNA sequence")
         return 1

     # initializes a flipped DNA variable
     dir = DNA['dir']
     if dir is True:
         DNA_flipped = {'seq': "", 'dir': False}
     else:
         DNA_flipped = {'seq': "", 'dir': True}

     # makes the flipped sequence
     for i in range(len(DNA['seq'])):
         DNA_flipped['seq'] += DNA['seq'][-(i + 1)]

     # returns the flipped sequence in a DNA dictionary construct
     return DNA_flipped

 def complementary(DNA):

      # ensures that a dictionary is given
     if type(DNA) is not dict:
         print("Not valid data structure")
         return 1

     # ensuress that DNA is a valid sequence
     if check(DNA) != 0:
         print("Not a valid DNA sequence")
         return 1

     # initializes a complementary DNA variable
     dir = DNA['dir']
     if dir is True:
         DNA_comp = {'seq': "", 'dir': False}
     else:
         DNA_comp = {'seq': "", 'dir': True}

     # makes the complementary sequence
     for i in range(len(DNA['seq'])):
         if DNA['seq'][i] == "a":
             DNA_comp['seq'] += "t"
         if DNA['seq'][i] == "t":
             DNA_comp['seq'] += "a"
         if DNA['seq'][i] == "c":
             DNA_comp['seq'] += "g"
         if DNA['seq'][i] == "g":
             DNA_comp['seq'] += "c"

     # returns the complementary sequence
     return DNA_comp
