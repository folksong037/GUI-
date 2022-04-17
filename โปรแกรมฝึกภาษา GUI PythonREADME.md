import string
from tkinter import * # Import tkinter Tkinter เป็นโมดูล อินเตอร์เฟซมาตรฐาน Python GUI ใช้ Tkinter สามารถสร้างโปรแกรม GUI
from tkinter import ttk, messagebox # ttk หรือ theme of Tk เป็นตัวใช้สำหรับ Design โปรแกรมและวิดเจ็ตบางตัวเป็นวิดเจ็ตเฉพาะสำหรับ ttk เช่น combobox , tk.messagebox (เด้งกล่องโต้ตอบการยืนยันข้อมูลคำเตือนและข้อผิดพลาด)
import sqlite3 # เป็นการ Import ข้อมูล Database
import random # เป็นการ Import เครื่องมือคำสังให้คำศัพท์ของเรา Random


############ DATABASE ###############

conn = sqlite3.connect(r"C:\Users\NITRO\Desktop\final sql.db") # สร้างตัวแปร conn เพื่อแทนที่เครื่องมือสำหรับติดต่อกับฐานข้อมูลที่มีชื่่อว่่า final sql.db 
c = conn.cursor() # คือเครื่องมือใช้สร้างตาราง ,เพิ่ม ,ลบ ,แก้ไข ,เรียกดูข้อมูลจากฐานข้อมูล
# c.execute คือการลงมือทำตามคำสั่งต่อไปนี้ 
# CREATE TABLE สร้างตารางที่มีชื่อว่า vocab ส่วน IF NOT EXISTS ถ้าตารางนี้ยังไม่มีให้สร้างขึ้นมาใหม่ แต่ถ้ามีแล้วไม่ต้องสร้างเพิ่ม
# ID Integer สร้างฟิลด์ชื่อ ID สำหรับเก็บข้อมูลเเต่ละชุด PRIMARY KEY AUTOINCREMENT คือ ให้ฟิลตัวนี้เป็นคีย์หลักและให้เพิ่มอัตโนมัติเมื่อมีการสร้างข้อมูลใหม่ขึ้นมา
# vocab,meaning text คือการสร้างฟิลด์ที่มีชื่อว่า vocab,meaning
c.execute("""CREATE TABLE IF NOT EXISTS Vocab ( 
		vocab text,
		meaning text)""")

# สร้างฟังก์ชันป้อนค่าเข้าไปใน Table หรือตาราง
def insert_vocab(vocab,meaning):
    with conn: # ใช้สำหรับเรียกฐานข้อมูลนั้นๆขึ้นมา
		# INSERT INTO vocab เพิ่มข้อมูลเข้าไปในตารางที่มีชื่อว่า vocab
		# VALUES (?,?) ใส่เครื่องหมาย ? แทนฟิลด์ทั้งหมด
        c.execute("""INSERT INTO vocab VALUES (?,?)""",
                  (vocab,meaning)) # คือตัวแปรที่เราจะนำเอามาใส่ในตาราง
    conn.commit() # เป็นคำสั่งสำหรับการบันทึกข้อมูล ต้องใช้ทุกครั้ง
    print('Data was inserted')

# สร้างฟังก์ชัน view_vocab
def view_vocab(): 
    with conn:
        c.execute("SELECT * FROM Vocab")
        allvocab = c.fetchall() # .fetchall ดึงข้อมูลทั้งหมดจาก database 
        print(allvocab)

    return allvocab # สร้างคำสั่ง return เพื่อนำค่าที่ได้ไปใช้งานต่อ


############ MAIN GUI ###############

GUI = Tk() # สร้าง GUI ขึ้นมา
GUI.geometry('600x550+500+150') # ขยายขนาดของ GUI และปรับตำแหน่งการแสดงผล
GUI.title('โปรแกรมฝึกภาษา เพื่อเพิ่มความแอคอาร์ตสมาร์ทด็อก') # ตั้งชื่อโปรแกรมของเรา
GUI.iconbitmap(r"C:\Users\NITRO\Downloads\pngegg-_5_.ico") # เปลี่ยนไอคอนของโปรแกรม

FONT1 = ('NINJA NARUTO',45) # กำหนด Font ให้สวยงาม
FONT2 = ('NINJA NARUTO',40)
FONT3 = ('ONE PIECE',10)
FONT4 = ('NINJA NARUTO',10)
FONT5 = ('Angsana New',55)
FONT6 = ('Angsana New',10)

######### NOTEBOOK ###########

Tab = ttk.Notebook(GUI) #ttk.Notebook คือสร้าง Tab ขึ้นมา (ในภาษา GUI เรียก Notebook)

T1 = Frame(Tab) # Fram เป็นโมดูลหนึ่งที่นำมาใช้เพื่อสร้างหน้าต่างขึ้นมา
T2 = Frame(Tab)
T3 = Frame(Tab)
T4 = Frame(Tab)

Tab.pack(fill=BOTH,expand=1) 

# Add ชื่อ Tab เข้าไป
Tab.add(T1,text='Add Vocab') 
Tab.add(T2,text='All Vocab')
Tab.add(T3,text='Flashcard')
Tab.add(T4,text='Spell')

############## IMAGE BG ###############
canvas = Canvas(T1, width = 600, height = 555)      
canvas.place(x=0,y=0,relwidth=1,relheight=1)

img = PhotoImage(file=r"C:\Users\NITRO\Downloads\cropped-600-555-320344 (1).png")    
canvas.create_image(0,0,anchor=NW,image=img)

canvas2 = Canvas(T2, width = 600, height = 555)      
canvas2.place(x=0,y=0,relwidth=1,relheight=1)

img2 = PhotoImage(file=r"C:\Users\NITRO\Downloads\cropped-600-555-727036 (4).png")    
canvas2.create_image(0,0,anchor=NW, image=img2)

canvas3 = Canvas(T3, width = 600, height = 555)      
canvas3.place(x=0,y=0,relwidth=1,relheight=1)

img3 = PhotoImage(file=r"C:\Users\NITRO\Downloads\wallpaperbetter (1).png")    
canvas3.create_image(0,0,anchor=NW, image=img3)

canvas4 = Canvas(T4, width = 600, height = 555)      
canvas4.place(x=0,y=0,relwidth=1,relheight=1)

img4 = PhotoImage(file=r"C:\Users\NITRO\Downloads\wallpaperflare-cropped (7).png")     
canvas4.create_image(0,0,anchor=NW, image=img4)

# *********** TAB 1 *************** #

######### VOCAB ###########
canvas.create_text(301,80, text=' VOCAB ! ',font=FONT1, fill='black')
canvas.create_text(300,80, text=' VOCAB ! ',font=FONT1, fill='white')

v_vocab = StringVar() # กำหนดค่าคลาสด้วย StringVar จากนั้นนำตัวแปร v_vocab ของคลาสดังกล่าวมาเชื่อมโยงกับ Entry ด้วยออปชัน textvariable 
E1 = ttk.Entry(T1,textvariable=v_vocab,font=FONT5,foreground='#a10007',width=18) # ttk.Entry สร้างกล่องข้อความไว้กรอกข้อมูล , textvariable ใช้ระบุตัวแปรชนิด StringVar เพื่อจัดการกับข้อความใน Entry
E1.pack()
E1.place(x=75,y=110)

######### MEANING ###########
canvas.create_text(301,260, text=' MEANING ',font=FONT1, fill='black')
canvas.create_text(300,260, text=' MEANING ',font=FONT1, fill='white')
v_meaning = StringVar() # กำหนดค่าคลาสด้วย StringVar จากนั้นนำตัวแปร v_meaning ของคลาสดังกล่าวมาเชื่อมโยงกับ Entry ด้วยออปชัน textvariable 
E2 = ttk.Entry(T1,textvariable=v_meaning,font=FONT5,foreground='#a10007',width=18) # ttk.Entry สร้างกล่องข้อความไว้กรอกข้อมูล , textvariable ใช้ระบุตัวแปรชนิด StringVar เพื่อจัดการกับข้อความใน Entry
E2.pack()
E2.place(x=75,y=290)

######### BUTTON ###########

# สร้างฟังก์ชันเพื่อเช็คภาษา
def isEnglish(s):
    try: 
        s.encode(encoding='utf-8').decode('ascii') # encoding "utf-8" เป็นภาษากลาง แปลงไปเป็น ascii (ascii จะมีเเค่ภาษาอังกฤษ และตัวอักษรพิเศษเท่านั้น)
    except UnicodeDecodeError:
        return False # except เป็นภาษาอื่นจะเท่ากับ False
    else:
        return True # หากเป็นภาษาอังกฤษจะเท่ากับ True
#

def SaveVocab(event=None): # event เป็นการส่ง event เข้าไปเมื่่อกด Enter , หากไม่มีการส่งข้อมูลอะไรเลย เราจะใช้ event = None เป็นการใส่ค่า default เมื่อกดปุ่มโดยใช้เมาส์
	global addvocab # ประกาศ global เพื่อจะนำเอาตัว addvocab ไปใช้งานในด้านอกหรือในฟังก์ชันไหนก็ได้ รวมถึงปุ่มต่างๆ
	vocab = v_vocab.get() # .get เป็นการดึงคำศัพท์มาจากตัว v_vocab Entry ให้ใช้เแทนคำว่า vocab แทน
	meaning = v_meaning.get() # .get เป็นการดึงคำศัพท์มาจากตัว v_meaning Entry ให้ใช้เแทนคำว่า meaning แทน          # meaning.isalpha() != True คือจะสามารถใส่ตัวสระต่างในภาษาไทยได้
	if isEnglish(vocab) and vocab.isalpha() == True and len(vocab) > 0 and len(meaning) > 0 and meaning.isalpha() != True and not(isEnglish(meaning)) : # not ไม่ต้องงาน เพราะเราต้องการภาษาไทย		
		#ส่งค่า vocab ไปเข้าไป isEnglish และต่อด้วย vocab.isalpha เพื่อทำการไม่ให้กรอกตัวอักษรพิเศษได้ สุดท้าย len สร้างขึ้นเพื่อป้องกันการบันทึกรัวๆ โดยให้ len สั่งนับว่ามีตัวอักษรที่กรอกลงไปมากกว่า 0 ตัว หรือเปล่า และ(and) ต้องเป็นจริงทั้งสอง ทั้ง vocab และ meaning
		insert_vocab(vocab,meaning) # นำตัวแปร vocab กับ  maening บันทึกข้อมูลลงใน Database
		print('V: {} M: {}'.format(vocab,meaning))  # ใช้คำสั่งปริ้นโดยใช้เมธอด format เพื่อดึง vocab กับ maening ออกมาปริ้น
		v_vocab.set('') #.set สร้างเพื่อเคลียร์ข้อมูลในตัวแปล v_vocab เมื่อกด Save vocab
		v_meaning.set('') #.set สร้างเพื่อเคลียร์ข้อมูลในตัวแปล v_meaning เมื่อกด Save vocab
		E1.focus() #.focus() คือชี้ตำแหน่งไปยังวัตถุที่ต้องการ สร้างเพื่อให้เคอร์เซอร์เริ่มกรอกข้อมูลใหม่ที่ตำแหน่ง E1 หรือ Entry Vocab

		#clear old data
		vocabtable.delete(*vocabtable.get_children())
		#จาก vocabtable ให้ทำการ delete ทั้งหมดเพื่อทำการเคลียร์ทั้ง Table

		#update table เพื่อให้ข้อมูลที่ save โชว์เป็นปัจุบัน
		addvocab = view_vocab()
		for v in addvocab: 
			vocabtable.insert('','end',value=v)
		print('----------')
		messagebox.showinfo('สำเร็จ','บันทึกสำเร็จ')
	    
	else: #ถ้านับไปแล้วเท่ากับ 0 จะโชว์ป๊อปอัพ messagebox ขึ้นมา
		messagebox.showinfo('เกิดข้อผิดพลาด','กรุณาตรวจสอบข้อมูลที่กรอกใหม่อีกครั้ง')

T1B1 = ttk.Button(T1,text='Save Vocab 🐼',command=SaveVocab)
T1B1.pack(ipadx=40,ipady=20,padx=30,pady=20)
T1B1.place(x=250,y=400)

E2.bind('<Return>',SaveVocab) # bind เป็นการผูกกับปุ่ม Return แล้วลิ้งไปที่ SaveVocab   สร้างเพื่อให้กดปุ่ม Enter เพื่อ Save เเทนการกดปุ่มแบบเมาส์
						
# *********** TAB 2 *************** #

header = ['Vocab','Meaning'] # กำหนด header หรือหัวข้อบนตาราง
hdsize = [200,250] # กำหนด ขนาดความกว้างของเเต่ละคอลัม

vocabtable = ttk.Treeview(T2,columns=header,show='headings',height=20) #สร้างตารางด้วยคำสั่ง ttk.Treeview ลงใน T2 กำหนดคอลัมต่างๆจากใน header และต้องการให้มีคำศัพท์ 20 คำในตาราง
vocabtable.place(x=70,y=50)

canvas2.create_text(115,27, text='เลือกคำศัพท์ที่ต้องการลบ',font=FONT3, fill='white') 

T2L1 = ttk.Entry(T2,font=FONT6,foreground='black',width=35)
T2L1.pack()
T2L1.place(x=200,y=17)

# ตัวแปร h แทน header และ s แทน hdsize ใช้คำสั่ง zip จับ header,hdsize มาคู่กัน คือ zip()
for h,s in zip(header,hdsize): # ฟังก์ชัน zip สามารถช่วยเรารวมข้อมูลใน list ตำแหน่งเดียวกัน และสามารถใช้ประโยชน์ได้ทั้งการแปลง zip object ให้อยู่ในรูปแบบของ List ที่เก็บ Tuple คู่ของข้อมูลไว้
	vocabtable.heading(h,text=h) #  h ตัวแรกคือheader h ตัวที่2คือ ชื่อหัวข้อ
	vocabtable.column(h,width=s) # สร้างขึ้นเพื่อควบคุมคอมลัมให้เท่ากับหัวข้อ


# สร้างฟังก์ชันสำหรับการลบคำศัพท์
def delete(): 
	T2L1.get() #รับข้อมูลจาก T2L1 
	c.execute(' DELETE FROM Vocab WHERE vocab=? ',(T2L1.get(),) ) #ไปหา vocab ในตาราง Vocab ว่าข้อมูลที่เรารับมาตรงกันไหมแล้วให้ลบ
	conn.commit()

def refresh():
    #จาก vocabtable ให้ทำการ delete ทั้งหมดเพื่อทำการเคลียร์ทั้ง Table
    vocabtable.delete(*vocabtable.get_children()) 

    #update table เพื่อให้ข้อมูลที่ save โชว์เป็นปัจุบัน
    addvocab = view_vocab() 
    for v in addvocab: 
        vocabtable.insert('','end',value=v)
    print('----------')

T2B1 = ttk.Button(T2,text='ลบ 🗑️',command=delete)
T2B1.pack(ipadx=10,ipady=10,pady=10)
T2B1.place(x=360,y=15)
T2B2 = ttk.Button(T2,text='รีเฟรช 🌀',command=refresh)
T2B2.pack(ipadx=10,ipady=10,pady=10)
T2B2.place(x=450,y=15)


# *************TAB 3 ***************** #
rv_vocab = StringVar() # Result Vocab 
rv_vocab.set('✨ VOCAB ✨') 
rv_check = StringVar() # Result Vocab Check คำตอบ
rv_check.set('THE ANSWER')

T3R1 = ttk.Label(T3,textvariable=rv_vocab,font=FONT2,background="Black",foreground='#3D53EE') # สร้าง Label สำหรับแสดงผล Result ในTab3 และใช้คำสั่ง textvariable เพราะจะมีการเปลี่ยนแปลงของผลลัพธ์
T3R1.pack(pady=20)

T3R2 = ttk.Label(T3,text='TRANSLATION',font=FONT2,background="Black",foreground='#41C1B4') # สร้าง Label เพื่อบอกให้ผู้ใช้บันทึกคำแปลตามคำศัพท์ที่แสดงด้านบน
T3R2.pack()

T3L1 = ttk.Entry(T3,font=FONT5,foreground='#532EC4',width=16) # สร้างกล่องข้อความสำหรับบันทึกคำแปล
T3L1.pack()

T3R2 = ttk.Label(T3,textvariable=rv_check,font=FONT2,background="Black",foreground='#E04D6E')  # สร้าง Label สำหรับแสดงผลของคำตอบ
T3R2.pack(pady=20)


# Button Result Fram3 เป็นการสร้างเฟรมสำหรับใส่ปุ่ม
BRF = Frame(T3)
BRF.pack()

# สร้างฟังก์ชันให้กับปุ่ม Next
def NextButton():
	global vcurrent # ประกาศ global เพื่อให้ vcurrent ไปแทนที่ตัวด้านนอก
	current_1 = random.choice(addvocab) # Random คำศัพท์ที่เรา Add เข้าไปใน Tabel # (2, 'Cat', 'แมว', 0) 
	vcurrent = current_1 # 
	# Set เป็นออบเจ็คที่ใช้สำหรับเก็บข้อมูลที่ไม่ซ้ำกัน ในการเขียนโปรแกรมเราสามารถใช้ Set สำหรับหาค่าที่ไม่ซ้ำกันภายในลิสต์ ตรวจสอบข้อมูลที่มีอยู่แล้ว หรือการดำเนินการทางคณิตศาสตร์เกี่ยวกับ Set ได้
	rv_vocab.set(current_1[0]) # set เข้าไปใน current_1 index ที่ 1 เพราะ คำศัพท์ของเราอยู่ในตำแหน่งที่ 1
	rv_check.set('THE ANSWER')
	
# สร้างฟังก์ชันให้กับปุ่ม Check
def CheckButton():
	kk = T3L1.get() # kk รับค่าจาก L11 ที่เรารับเข้ามา
	if kk == vcurrent[1]: # ถ้าใน kk กรอกคำตอบถูกตรงกับ vcurrent ในตำแหน่งที่ 1
		rv_check.set("CORRECT ✔️") # จะแสดงคำว่า CORRECT
	else:
		rv_check.set("INCORRECT ❌") # แต่ถ้าผิด จะแสดงคำว่า INCORRECT

T3BR1 = ttk.Button(BRF,text='Next',command=NextButton) # ปุ่มนี้จะเป็นปุ่ม Next เพื่อ Random คำศัพท์
T3BR1.grid(row=0,column=0,ipadx=40,ipady=20,padx=10)
T3BR2 = ttk.Button(BRF,text='Check',command=CheckButton)
T3BR2.grid(row=0,column=1,ipadx=40,ipady=20,padx=10)   # ปุ่มนี้จะเป็นปุ่ม Check เพื่อ Check คำตอบ 

# ********** TAB 4 ************** #

spell_vocab = StringVar()
spell_vocab.set('👑 VOCAB 👑')
spell_check = StringVar()
spell_check.set('THE ANSWER')

T4R1 = ttk.Label(T4,textvariable=rv_vocab,font=FONT2,background="#922c44",foreground='#fb6006')
T4R1.pack(pady=20)

T4R2 = ttk.Label(T4,text='SPELLING',font=FONT2,background="#922c44",foreground='white')
T4R2.pack()

T4L1 = ttk.Entry(T4,font=FONT5,foreground='#2e80b0',width=16)
T4L1.pack()

T4R3 = ttk.Label(T4,textvariable=spell_check,font=FONT2,background="#922c44",foreground='#050404')
T4R3.pack(pady=20)

BRF_2 = Frame(T4)
BRF_2.pack()

vspell = []

def NextButton_2():
	global vspell
	current_2 = random.choice(addvocab) # ('Cat', 'แมว', )
	vspell = current_2
	rv_vocab.set(current_2[1])
	spell_check.set('THE ANSWER')

def CheckButton_2():
	spell_1 = T4L1.get() 
	if spell_1 == vspell[0]:
		spell_check.set("👍 CORRECT")
	else:
		spell_check.set("INCORRECT 👎")

T4BR1 = ttk.Button(BRF_2,text='Next',command=NextButton_2)
T4BR1.grid(row=0,column=0,ipadx=40,ipady=20,padx=10)
T4BR2 = ttk.Button(BRF_2,text='Check',command=CheckButton_2)
T4BR2.grid(row=0,column=1,ipadx=40,ipady=20,padx=10)

# ************Initial Program***************** #

global addvocab # ประกาศ global เพื่อจะนำเอาตัว addvocab ไปใช้ในฟังก์ชันไหนก็ได้ รวมถึงปุ่มต่างๆ
addvocab = view_vocab() # ดึง vocab ทั้งหมด ณ ปัจจุบันลงไปใน Tabel

if len(addvocab) > 0: # if len(addvocab) > 0 หากมีคำศัพท์มากกว่า 1 ตัว เราจะทำการรัน forloop
	for v in addvocab:
		vocabtable.insert('','end',value=v)

GUI.resizable(False, False) #ล็อคหน้าต่าง
GUI.mainloop()
