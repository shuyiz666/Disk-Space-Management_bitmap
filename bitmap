from tkinter import *
import time
import tkinter.messagebox as messagebox
import tkinter.font as tkFont

class BitmapData:
    def __init__(self, cylinder=8, trackNum=2, trackLength=4):
        self.__MAP = []
        self.__track = []
        self.__trackNum = trackNum
        self.__trackLength = trackLength
        self.__cylinder = cylinder

    def initMAP(self):
        self.__MAP = []
        self.__track = [0 for i in range(self.__trackLength)]
        for i in range(self.__cylinder):
            self.__MAP.append([])

        for i in range(len(self.__MAP)):
            for j in range(self.__trackNum):
                self.__MAP[i].append(self.__track)


    def displayMap(self):
        return self.__MAP

    def setMap(self, i, j, k):
        self.__MAP[i][j][k] = 1

    def askForPlot(self):
        # display on the GUI
        print("----------------------------------")
        for i in range(self.__cylinder):
            print("Cylinder", i, end="\t")
            for j in range(self.__trackNum):
                for k in range(self.__trackLength):
                    print(self.__MAP[i][j][k], end=" ")
                if j == self.__trackNum - 1:
                    print()
        print("----------------------------------")

    def askForAllocation(self, decAddress):

        self.allo_cylinder = decAddress // 8
        self.allo_track = ((decAddress % 8) // 4)
        self.allo_record = decAddress % 8 % 4

        if self.__MAP[self.allo_cylinder][self.allo_track][self.allo_record] == 0:
            addRecord = list(tuple(self.__MAP[self.allo_cylinder][self.allo_track]))
            addRecord[self.allo_record] = 1
            self.__MAP[self.allo_cylinder][self.allo_track] = addRecord
            print("Successfully Allocated...")
            print("Cylinder Num:", self.allo_cylinder)
            print("Track Num:", self.allo_track)
            print("Record Num:", self.allo_record)
            print("The address :", decToBin(self.allo_cylinder,3), decToBin(self.allo_track,1), decToBin(self.allo_record,2))

        else:
            print("This address has been occupied...")

    def isOccupy(self, decAddress):
        self.allo_cylinder = decAddress // 8
        self.allo_track = ((decAddress % 8) // 4)
        self.allo_record = decAddress % 8 % 4
        if self.__MAP[self.allo_cylinder][self.allo_track][self.allo_record] == 1:
            return False
        else:
            return True


    def askForDelete(self, decAddress):
        free_cylinder = decAddress // 8
        free_track = ((decAddress % 8) // 4)
        free_record = decAddress % 8 % 4
        if self.__MAP[free_cylinder][free_track][free_record] == 1:
            deleteRecord = list(tuple(self.__MAP[free_cylinder][free_track]))
            deleteRecord[free_record] = 0
            self.__MAP[free_cylinder][free_track] = deleteRecord
            print("Successfully Freed up...")
            print("Cylinder Num:", free_cylinder)
            print("Track Num:", free_track)
            print("Record Num:", free_record)
            print("The address :", decToBin(free_cylinder,3), decToBin(free_track,1), decToBin(free_record,2))
        else:
            print("This address has already been released...")
        return 0


class GUI1():
    def __init__(self, MAP:BitmapData):
        self.map1 = MAP

        root = Tk()
        root.title("Manual Disk Management")

        # frame1: label and input box
        frame1 = Frame(root)
        frame1.pack()

        # frame2: button
        frame2 = Frame(root)
        frame2.pack()

        # frame3: display box
        frame3 = Frame(root)
        frame3.pack()

        # frame4: log
        frame4 = Frame(root)
        frame4.pack()

        # create input box
        input = Label(frame1, text="Input")
        input.grid(row=0, column=0)
        self.entry = Entry(frame1)
        self.entry.grid(row=0, column = 1)

        # Create button
        cs = Button(frame2, text='check status', command=self.checkstatus)
        cs.grid(row=10,column=3)
        ac = Button(frame2, text='allocate', command = self.allocate)
        ac.grid(row=10,column=0)
        rl = Button(frame2, text='release', command = self.release)
        rl.grid(row=10, column=1)
        rs = Button(frame2, text='reset', command = self.reset)
        rs.grid(row=10, column=2)

        # create display box
        self.Cylinder0 = Label(frame3)
        self.Cylinder1 = Label(frame3)
        self.Cylinder2 = Label(frame3)
        self.Cylinder3 = Label(frame3)
        self.Cylinder4 = Label(frame3)
        self.Cylinder5 = Label(frame3)
        self.Cylinder6 = Label(frame3)
        self.Cylinder7 = Label(frame3)
        self.Cylinder0.pack()
        self.Cylinder1.pack()
        self.Cylinder2.pack()
        self.Cylinder3.pack()
        self.Cylinder4.pack()
        self.Cylinder5.pack()
        self.Cylinder6.pack()
        self.Cylinder7.pack()

        # create log
        self.log1 = Text(frame4)
        self.log1.grid(row=0, column=0)


        root.mainloop()


    # input error massage box
    def message(self):
        if len(self.entry.get()) > 6 or len(self.entry.get()) == 0 or self.entry.get().count('0')+self.entry.get().count('1') != len(self.entry.get()):
            messagebox.showerror("WARNING!", "Invalid input, please try again")
            self.flag = False

    # preprocess result
    def calculate(self, n):
        result = self.map1.displayMap()
        Cylinder = result[n][0] + result[n][1]
        return str(Cylinder)

    # status display
    def status(self):
        self.Cylinder0.configure(text='Cylinder0' + self.calculate(0))
        self.Cylinder1.configure(text='Cylinder1' + self.calculate(1))
        self.Cylinder2.configure(text='Cylinder2' + self.calculate(2))
        self.Cylinder3.configure(text='Cylinder3' + self.calculate(3))
        self.Cylinder4.configure(text='Cylinder4' + self.calculate(4))
        self.Cylinder5.configure(text='Cylinder5' + self.calculate(5))
        self.Cylinder6.configure(text='Cylinder6' + self.calculate(6))
        self.Cylinder7.configure(text='Cylinder7' + self.calculate(7))


    # button cs
    def checkstatus(self):
        self.status()

    # button ac
    def allocate(self):
        self.flag = True
        self.message()
        if self.flag:
            if not self.map1.isOccupy(BinToInt(self.entry.get(), len(self.entry.get()))):
                self.log1.insert(END, "Failed to allocate...\n"
                                 + "The address "
                                 + str(decToBin(self.map1.allo_cylinder, 3))
                                 + ' '
                                 + str(decToBin(self.map1.allo_track, 1))
                                 + ' '
                                 + str(decToBin(self.map1.allo_record, 2))
                                 + " has been occupied.\n\n")
                messagebox.showerror("WARNING!", "The address has been occupied")
                time.sleep(0.01)
                self.log1.update()
            else:
                self.map1.askForAllocation(BinToInt(self.entry.get(), len(self.entry.get())))
                self.log1.insert(END, "Successfully allocated...\n"
                                 + "Cylinder Num: " + str(self.map1.allo_cylinder)
                                 + "\nTrack Num: " + str(self.map1.allo_track)
                                 + "\nRecord Num: " + str(self.map1.allo_record)
                                 + "\nThe address: "
                                 + str(decToBin(self.map1.allo_cylinder,3)) + ' '
                                 + str(decToBin(self.map1.allo_track,1)) + ' '
                                 + str(decToBin(self.map1.allo_record,2)) + '\n\n')
                time.sleep(0.01)
                self.log1.update()
                self.status()

    # button rl
    def release(self):
        self.flag = True
        self.message()
        if self.flag:
            if self.map1.isOccupy(BinToInt(self.entry.get(), len(self.entry.get()))):
                self.log1.insert(END, "Failed to release...\n"
                                 + "The address "
                                 + str(decToBin(self.map1.allo_cylinder, 3))
                                 + ' '
                                 + str(decToBin(self.map1.allo_track, 1))
                                 + ' '
                                 + str(decToBin(self.map1.allo_record, 2))
                                 + " was originally empty.\n\n")
                time.sleep(0.01)
                self.log1.update()
                messagebox.showerror("WARNING!", "The address was originally empty")
            else:
                self.map1.askForDelete(BinToInt(self.entry.get(), len(self.entry.get())))
                self.log1.insert(END, "Successfully released...\n"
                                 + "Cylinder Num: " + str(self.map1.allo_cylinder)
                                 + "\nTrack Num: " + str(self.map1.allo_track)
                                 + "\nRecord Num: " + str(self.map1.allo_record)
                                 + "\nThe address: "
                                 + str(decToBin(self.map1.allo_cylinder, 3)) + ' '
                                 + str(decToBin(self.map1.allo_track, 1)) + ' '
                                 + str(decToBin(self.map1.allo_record, 2)) + '\n\n')
                time.sleep(0.01)
                self.log1.update()
                self.status()

    # button rs
    def reset(self):
        self.log1.delete('1.0', 'end')
        self.map1.initMAP()
        self.status()


class GUI2():
    def __init__(self, MAP:BitmapData):
        self.map1 = MAP

        root = Tk()
        root.title("Automatic Disk Management")

        # frame1: label and input box
        frame1 = Frame(root)
        frame1.pack()

        # frame2: checkbox
        frame2 = Frame(root)
        frame2.pack()

        # frame3: submit input button
        frame3 = Frame(root)
        frame3.pack()

        # frame4: func button
        frame4 = Frame(root)
        frame4.pack()

        # frame5: display box
        frame5 = Frame(root)
        frame5.pack()

        # frame6: log
        frame6 = Frame(root)
        frame6.pack()

        # create input label and box
        input = Label(frame1, text="Input")
        input.grid(row=0, column=0)
        self.entry = Entry(frame1)
        self.entry.grid(row=0, column=1)

        # create checkbox
        self.checkbox = IntVar()
        self.checkbox.set(0)
        FCFS = Radiobutton(frame2, text="FCFS", variable=self.checkbox, value=1)
        SSTF = Radiobutton(frame2, text="SSTF", variable=self.checkbox, value=2)
        SCAN = Radiobutton(frame2, text="SCAN", variable=self.checkbox, value=3)
        FCFS.grid(row=0, column=0)
        SSTF.grid(row=0, column=1)
        SCAN.grid(row=0, column=2)

        # create submit button
        submit = Button(frame3, text = 'submit', command=self.submit)
        submit.grid(row=0,column=3)

        # Create button
        cs = Button(frame4, text='check status', command=self.checkstatus)
        cs.grid(row=10,column=3)
        ac = Button(frame4, text='allocate', command=self.allocate)
        ac.grid(row=10,column=0)
        rl = Button(frame4, text='release', command=self.release)
        rl.grid(row=10, column=1)
        rs = Button(frame4, text='reset', command = self.reset)
        rs.grid(row=10, column=2)

        # create display box
        self.Cylinder0 = Label(frame5)
        self.Cylinder1 = Label(frame5)
        self.Cylinder2 = Label(frame5)
        self.Cylinder3 = Label(frame5)
        self.Cylinder4 = Label(frame5)
        self.Cylinder5 = Label(frame5)
        self.Cylinder6 = Label(frame5)
        self.Cylinder7 = Label(frame5)
        # self.Cylinder0.grid(row=0, column=0)
        # self.Cylinder1.grid(row=1, column=0)
        # self.Cylinder2.grid(row=2, column=0)
        # self.Cylinder3.grid(row=3, column=0)
        # self.Cylinder4.grid(row=4, column=0)
        # self.Cylinder5.grid(row=5, column=0)
        # self.Cylinder6.grid(row=6, column=0)
        # self.Cylinder6.grid(row=7, column=0)
        self.Cylinder0.pack()
        self.Cylinder1.pack()
        self.Cylinder2.pack()
        self.Cylinder3.pack()
        self.Cylinder4.pack()
        self.Cylinder5.pack()
        self.Cylinder6.pack()
        self.Cylinder7.pack()

        # create log
        self.log = Text(frame6)
        self.log.grid(row=0, column=0)

        # create submit list
        self.submitlist = []
        # create list store log
        self.logoutput = []

        root.mainloop()

    # submit button
    def submit(self):
        self.inputlist = self.entry.get().split(',')
        self.message()
        if self.checkbox.get() == 1: # FCFS
            self.submitlist = FCFS(self.inputlist)
        elif self.checkbox.get() == 2: # SSTF
            self.submitlist = SSTF(self.inputlist)
        elif self.checkbox.get() == 3: # SCAN
            self.submitlist = SCAN(self.inputlist)
        else:
            messagebox.showerror("WARNING!","Please choose a scheduling method")
        # self.entry.delete(0,'end')  # empty input


    # input error massage box
    def message(self):
        for str in self.inputlist:
            if len(str) > 6 or len(str) == 0 or str.count('0')+str.count('1') != len(str):
                messagebox.showerror("WARNING!", "Invalid input, please try again")

    # preprocess result
    def calculate(self, n):
        result = self.map1.displayMap()
        Cylinder = result[n][0] + result[n][1]
        return str(Cylinder)
    #
    # status display
    def status(self):
        self.Cylinder0.configure(text='Cylinder0' + self.calculate(0))
        self.Cylinder1.configure(text='Cylinder1' + self.calculate(1))
        self.Cylinder2.configure(text='Cylinder2' + self.calculate(2))
        self.Cylinder3.configure(text='Cylinder3' + self.calculate(3))
        self.Cylinder4.configure(text='Cylinder4' + self.calculate(4))
        self.Cylinder5.configure(text='Cylinder5' + self.calculate(5))
        self.Cylinder6.configure(text='Cylinder6' + self.calculate(6))
        self.Cylinder7.configure(text='Cylinder7' + self.calculate(7))

    # button cs
    def checkstatus(self):
        self.status()


    # button ac
    def allocate(self):
        global logcontent
        if not self.submitlist:
            messagebox.showerror("WARNING!","Please submit your input")
        else:
            self.log.insert(END, "scheduling queue: " + str(self.submitlist) + "\n\n")
            for STR in self.submitlist:
                if not self.map1.isOccupy(BinToInt(STR, len(STR))): # The address is occupied
                    self.log.insert(END, "Failed to allocate...\n"
                                    + "The address "
                                    + str(decToBin(self.map1.allo_cylinder,3)) + ' '
                                    + str(decToBin(self.map1.allo_track,1)) + ' '
                                    + str(decToBin(self.map1.allo_record,2))
                                    + " has been occupied.\n\n")
                    time.sleep(0.01)
                    self.log.update()
                    messagebox.showerror("WARNING!", "The address has been occupied")
                else:
                    self.map1.askForAllocation(BinToInt(STR, len(STR)))
                    self.log.insert(END, "Successfully allocated...\n"
                                    + "Cylinder Num: " + str(self.map1.allo_cylinder)
                                    + "\nTrack Num: " + str(self.map1.allo_track)
                                    + "\nRecord Num: " + str(self.map1.allo_record)
                                    + "\nThe address: "
                                    + str(decToBin(self.map1.allo_cylinder,3)) + ' '
                                    + str(decToBin(self.map1.allo_track,1)) + ' '
                                    + str(decToBin(self.map1.allo_record,2)) + '\n\n')
                    time.sleep(0.01)
                    self.log.update()
                    self.status()
            self.submitlist = []

    # button rl
    def release(self):
        if not self.submitlist:
            messagebox.showerror("WARNING!","Please submit your input")
        else:
            self.log.insert(END, "scheduling queue: " + str(self.submitlist) + "\n\n")
            for STR in self.submitlist:
                if self.map1.isOccupy(BinToInt(STR, len(STR))):
                    self.log.insert(END, "Failed to release...\n"
                                    + "The address "
                                    + str(decToBin(self.map1.allo_cylinder,3)) + ' '
                                    + str(decToBin(self.map1.allo_track,1)) + ' '
                                    + str(decToBin(self.map1.allo_record,2))
                                    + " was originally empty.\n\n")
                    time.sleep(0.01)
                    self.log.update()
                    messagebox.showerror("WARNING!", "The address was originally empty")
                else:
                    self.map1.askForDelete(BinToInt(STR, len(STR)))
                    self.log.insert(END, "Successfully released...\n"
                                    + "Cylinder Num: " + str(self.map1.allo_cylinder)
                                    + "\nTrack Num: " + str(self.map1.allo_track)
                                    + "\nRecord Num: " + str(self.map1.allo_record)
                                    + "\nThe address: "
                                    + str(decToBin(self.map1.allo_cylinder, 3)) + ' '
                                    + str(decToBin(self.map1.allo_track, 1)) + ' '
                                    + str(decToBin(self.map1.allo_record, 2)) + '\n\n')
                    time.sleep(0.01)
                    self.log.update()
                    self.status()
            self.submitlist = []

    # button rs
    def reset(self):
        self.map1.initMAP()
        self.log.delete('1.0', 'end')
        self.status()
        self.submitlist = []


class window():
    def __init__(self):
        # self.map1 = MAP

        root = Tk()
        root.title("Welcome")


        # frame1: welcome
        frame1 = Frame(root)
        frame1.pack()

        # frame2: checkbox
        frame2 = Frame(root)
        frame2.pack()

        # frame3: submit button
        frame3 = Frame(root)
        frame3.pack()


        # create welcome label
        ft = tkFont.Font(family='Fixdsys', size=20, weight=tkFont.BOLD)
        welcome = Label(frame1, text="Welcome to disk management system", font=ft)
        welcome.grid(row=0, column=0,padx=200, pady=100)

        # create checkbox
        self.windowcheckbox = IntVar()
        self.windowcheckbox.set(0)
        manual = Radiobutton(frame2, text="Manual Operation", variable=self.windowcheckbox, value=1)
        auto = Radiobutton(frame2, text="Automatic Operation", variable=self.windowcheckbox, value=2)
        manual.grid(row=0, column=0,padx=100,pady=20)
        auto.grid(row=0, column=1,padx=100,pady=20)

        #create submit button
        windowsubmit = Button(frame3, text='submit', command=self.windowsubmit_func)
        windowsubmit.grid(row=0,column=3,pady=50)


        root.mainloop()

    def windowsubmit_func(self):
        return self.windowcheckbox.get()
        # if self.windowcheckbox.get() == 1: # Manual
        #     GUI1(self.map1)
        # elif self.windowcheckbox.get() == 2: #auto
        #     GUI2(self.map1)
        #
        # else:
        #     messagebox.showerror("WARNING!", "Please choose a operation type.")




def decToBin(decimal, digit):
    fitZero = "0" * digit
    return fitZero[:(len(fitZero)-len(str(bin(decimal))[2:]))] + str(bin(decimal))[2:]


def BinToInt(num: str, binary: int):
    bitCount = binary
    number = 0
    while bitCount > 0:
        if num[bitCount - 1] == "1":
            number += pow(2, binary - bitCount)
        bitCount -= 1
    return number

def FCFS(inp:list):
    output = inp[:]
    return output

def SSTF(inp:list):
    pos = BinToInt("11111", len("11111"))
    res = []
    dec_inp = []
    for i in inp:
        dec_inp.append(BinToInt(i, len(i)))
    while len(dec_inp) !=0:
        res1 = []
        for i in dec_inp:
            res1.append(abs(i-pos))
        min = res1[0]
        for i in range(len(res1)):
            if res1[i] <= min:
                min = res1[i]
        res.append(dec_inp[res1.index(min)])
        dec_inp.pop(res1.index(min))
        pos = res[-1]
    res_ = []
    for i in res:
        res_.append(bin(i)[2:])
    return res_

def SCAN(inp:list):
    pos = BinToInt("11111", len("11111"))
    dec_inp = []
    for i in inp:
        dec_inp.append(BinToInt(i, len(i)))
    distance = []
    for i in dec_inp:
        distance.append(i-pos)
    dic = {}
    for i in range(len(dec_inp)):
        dic[distance[i]] = dec_inp[i]
    res1 = []
    res2 = []
    for i in sorted(dic):
        if i < 0:
            res1.append(i)
        elif i >= 0:
            res2.append(i)
    res1.sort(reverse=True)
    res2.sort()
    res3 = res1+res2
    res = []
    for i in res3:
        res.append(dic.get(i))
    res_ = []
    for i in res:
        res_.append(bin(i)[2:])
    return res_

def main():
    map1 = BitmapData()
    map1.initMAP()
    isContinue = True
    res = window().windowsubmit_func()
    print(res)
    if res == 1:
        GUI1(map1)
    if res == 2:
        GUI2(map1)

main()



