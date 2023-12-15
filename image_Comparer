import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
import time
import functools
from collections import Counter
from tkinter import TOP, Entry, Label, StringVar, ttk
from tkinterdnd2 import *

#creates file to compare to

def createExamples():
    #creates text file to be appended
    numberArrayExamples = open("numArEx.txt", "a")
    numbersWeHave = range(0, 10)
    versionsWeHave = range(1, 10)

    #opens each example image, converts it into an array, then adds the array to a text file
    for eachNum in numbersWeHave:
        for eachVer in versionsWeHave:
            #print(str(eachNum)+"."+str(eachVer))
            imageFilePath = r"C:\\Users\\tywhi_phn55uu\\Downloads\\tutorialimages\\images\\numbers\\"+ str(eachNum)+"."+str(eachVer)+".png"
            ei  = Image.open(imageFilePath)
            eiar = np.array(ei)
            eiar1 = str(eiar.tolist())

            lineToWrite = str(eachNum)+'::'+eiar1+'\n'
            numberArrayExamples.write(lineToWrite)



#takes a pixels rgb values, averages them, then takes the average of every pixel and averages that
#then if each individual pixel's rgb value is above the "every pixel average", the pixel is changed to white, and if it's below it changes to black.

def threshhold(imageArray):
    balanceAr = []
    newAr = imageArray

    for eachRow in imageArray:
        for eachPix in eachRow:
            avgNum = np.mean(eachPix[:3])
            balanceAr.append(avgNum)
    balance = np.mean(balanceAr)       

    for eachRow in newAr:
        for eachPix in eachRow:
            if functools.reduce(lambda x, y: x + y, eachPix[:3]/len(eachPix[:3])) > balance:

                eachPix[0]  = 255
                eachPix[1]  = 255
                eachPix[2]  = 255
                eachPix[3]  = 255  

            else:
                eachPix[0]  = 0
                eachPix[1]  = 0
                eachPix[2]  = 0
                eachPix[3]  = 255

    return newAr             


#1.opens a text file with the arrays to compare 2.splits the text file by line 3.Opens the image to compare to
#4.Converts the image into an array 5.Splits the text file into IDs and data with "::" 
#6.Picks out one pixel from the text file and one pixel from the image array and compares them
#Counts the number of pixel matches that occur for each number in the text file

def whatNumIsThis(filePath):
    matchedAr = []
    loadExamps = open('numArEx.txt', "r").read()
    loadExamps = loadExamps.split("\n")

    i = Image.open(filePath)
    iar = np.array(i)
    iarl = iar.tolist()

    inQuestion = str(iarl)

    for eachExample in loadExamps:
        if len(eachExample) > 3:
            splitEx = eachExample.split("::")
            currentNum = splitEx[0]
            currentAr = splitEx[1]

            eachPixEx = currentAr.split("],")

            eachPixInQ = inQuestion.split("],")

            x=0

            while x < len(eachPixEx):
                if eachPixEx[x] == eachPixInQ[x]:
                    matchedAr.append(int(currentNum))

                x += 1

    x = Counter(matchedAr)    
    print(x) 

    graphX = []
    graphY = []

    for eachThing in x:
        graphX.append(eachThing)
        graphY.append(x[eachThing])

    fig = plt.figure()
    ax1 = plt.subplot2grid((4,4), (0,0), rowspan=1, colspan=4)   
    ax2 = plt.subplot2grid((4,4), (1,0), rowspan=1, colspan=4)
    
    ax1.imshow(iar)
    ax2.bar(graphX, graphY, align="center")
    plt.ylim(380)

    xloc = plt.MaxNLocator(11)

    ax2.xaxis.set_major_locator(xloc)

    plt.show()

def get_path(event):
    global photoPath
    pathLabel.configure(text = event.data)
    photoPath = event.data
    root.destroy()

root = TkinterDnD.Tk()
root.geometry("350x140")
root.title("Number Identifier")

nameVar = StringVar()

#create text box
entryWidget = Entry(root)
entryWidget.pack(ipadx = 100, ipady=40,  side=TOP, padx=5, pady=5)

#Creates path label
pathLabel = Label(root, text="Drag and drop file in the entry box")
pathLabel.pack(side=TOP)

#allows DnD in text box 
entryWidget.drop_target_register(DND_ALL)

#gets DnDed files path
entryWidget.dnd_bind("<<Drop>>", get_path)

root.mainloop() 

print(photoPath)

whatNumIsThis(photoPath)
    
