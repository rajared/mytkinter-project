# mytkinter-project
tkinter project
# This is a sample Python script.

# Press Shift+F10 to execute it or replace it with your code.
# Press Double Shift to search everywhere for classes, files, tool windows, actions, and settings.
from tkinter import *
import tkinter.messagebox
# This is a sample Python script.

# Press Shift+F10 to execute it or replace it with your code.
# Press Double Shift to search everywhere for classes, files, tool windows, actions, and settings.
from tkinter import *
import tkinter.messagebox
import pandas as pd
from pandas import *
from os.path import exists
special_characters = ['@', '~', '.', ' ', '~', '!', '#', '$', '%', '^', '*', '(', ')', '-', '_', ',', '/', ]
special_characters1 = ['~',' ', '~', '!', '#', '$', '%', '^', '*', '(', ')', '-', '_', ',', '/', ]

print("purly its based on core python ,this program  fully guided with instruction in the console you just follow the instruction")


def button_click():

    '''''all function return values '''
    validinputs=[validate_nam(),validate_ema(),validate_psn(),
                 validate_loc(),validate_des()]

    if False not in validinputs:
        '''converting request value number to description'''
        if var.get()==1:
            request='IT'
        else:
            request='HR'


        'coverting psno to integer '
        box_psn2=int(box_psn.get())

        '''creating dictionary with textbox values '''
        dic_of_textboxes={ 'name':[box_name.get()],
                          'psno' :[box_psn2],
                          'email':[box_ema.get()],
                          'request':[request],
                           'location':[box_loc.get()],
                          'discriptiion':[box_des.get()]
                           }

        '''message info window'''
        tkinter.messagebox.showinfo("1st click",dic_of_textboxes)

        '''if file already exist append to excel'''
        if exists('ltts.xlsx')==True:
            df = pd.read_excel('ltts.xlsx',engine='openpyxl')
            global list_name,list_psno,list_email,psno

            ''''get list of names, psno,email for validation'''
            list_name=df['name'].to_list()
            list_psno=df['psno'].to_list()
            list_email=df['email'].to_list()

            ''''conditions for not be duplicate values'''
            if box_name.get() in list_name:
                print('name already their, plz give another name')
            elif box_psn2 in list_psno:
                print('psno already present ,plz give another psno')
            elif box_ema.get() in list_email:
                print('email already prasent,plz provide another mailid')
            else:

                '''if all condtions ware satisfied create dataframe1 by reading excel file and append new dictionary df2 to df1 and append to excel '''
                df1 = pd.DataFrame(dic_of_textboxes)
                dff = df.append(df1, ignore_index=True)

                '''removing unnamed columns'''
                dff.drop(dff.columns[dff.columns.str.contains('unnamed', case=False)], axis=1, inplace=True)
                dff.to_excel('ltts.xlsx',engine='openpyxl')
                print(dff)
                print("you have successfully registered")

        else:
            ''''if file not present create new'''
            f=open('ltts.xlsx','x')
            f.close()

            ''''append record to excel'''
            df=pandas.DataFrame(dic_of_textboxes)
            df.to_excel('ltts.xlsx',engine='openpyxl')
            print(df)


'''validating name with pure python'''
def validate_nam():
    name11=box_name.get()
    for i in range(len(name11)):
        if name11[i].isdigit():
            print('name ,should not have numbers')
            return False

    if len(name11)<4:
         print('name must have minimum 4chars ')
         return False
    elif name11=='':
        print('name not be empty ,plz enter ur name')
        return False
    elif any(c in special_characters for c in name11):
        print("name special chars or spaces not allowed yes")
        return False
    else:
       return True

'''Complete ltts standard email '''
def validate_ema():
    email1=box_ema.get()
    email2=email1.split('@')
    email3=email2[0].split('.')
    str2=email1.endswith('@ltts.com')
    count=email1.count('.')
    for i in range(len(email1)):
        if email1[i].isdigit():
            print('email ,should not have numbers')
            return False
            break

    if str2 != True:
        print('mail not ends with @ltts.com,plz check missing')
        return False
    elif any(c in special_characters1 for c in email1):
        print("email special chars or spaces not allowed yes")
        return False
    elif count!=2:
        print('its not ltts mail which have 2 or only 2 dots ')
        return False
    elif email3[1]=='':
        print('mail should have few chars after first dot .')
        return False
    else:
        return True


'''validate psn by length '''
def validate_psn():
    psn=int(box_psn.get())
    if len((box_psn.get()))!= 8:
        print('psn,limit 8 digits ')
        return False
    elif type(psn)==str:
        print('should not be string ,plz enter numbers')
        return False
    else:
        return True

'''validate location from list defined below'''

def validate_loc():
    locations=['hyderabad','banglore','mysore','mumbai']
    if box_loc.get() in locations:
        return True
    else:
        print('location must be an entry in the location list')
        return False


'''validate description should not be empty and numbers'''
def validate_des():
    des=box_des.get()
    if box_des.get()!='':
        if des.isdigit():
            print('description must be chara ,plz enter  charecters')
            return False
        elif any(c in special_characters for c in des):
            print("desc not allowed yes special chars or spaces")
            return False
        else:
            return True
    else:
        print('description must not be empty')
        return False




'''''main window definition'''
window=tkinter.Tk()
window.title("Registration")
window.geometry('600x600')


'''''definition for lable and entry of name'''
text_nam = StringVar()
box_name = StringVar(window,value='rajavardhanreddy')
lable1=Label(window,textvar=text_nam)
text_nam.set('name')
lable1.grid(row=0,column=5)
text_box=Entry(window, textvar=box_name,validate="focusout", validatecommand=validate_nam)
text_box.grid(row=0,column=6)



'''''definition for lable and entry of email'''
text_ema = StringVar()
lemail=Label(window,textvar=text_ema)
text_ema.set('email')
lemail.grid(row=1,column=5)
box_ema=StringVar(window,value='guttikondarajavardhan.reddy@ltts.com')
box_ema=Entry(window,textvar=box_ema,validate="focusout", validatecommand=validate_ema)
box_ema.grid(row=1,column=6)


'''''definition for lable and entry of psnumber'''
text_psn = IntVar()
lpsno=Label(window,textvar=text_psn)
text_psn.set('psnumber')
lpsno.grid(row=2,column=5)
box_psn=IntVar(window,value=99005559)
box_psn=Entry(window,textvar=box_psn,validate="focusout", validatecommand=validate_psn)
box_psn.grid(row=2,column=6)


'''''definition for lable and entry of location'''
text_loc = StringVar()
lloc=Label(window,textvar=text_loc)
text_loc.set('location')
lloc.grid(row=3,column=5)
box_loc=StringVar(window,value='hyderabad')
box_loc=Entry(window,textvar=box_loc,validate="focusout", validatecommand=validate_loc)
box_loc.grid(row=3,column=6)


'''''definition for request choices output '''
def viewSelected():
    choice = var.get()
    if choice == 1:
        output = "IT"
    elif choice == 2:
        output = "HR"
    else:
        output = "Invalid selection"
    return tkinter.messagebox.showinfo('PythonGuides', f'You Selected {output}.')


'''''definition for lable and entry of Request'''
var = IntVar()
Radiobutton(window, text="IT", variable=var, value=1, command=viewSelected).grid(row=4,column=6)
Radiobutton(window, text="HR", variable=var, value=2, command=viewSelected).grid(row=4,column=7)
text_req = StringVar()
lreq=Label(window,textvar=text_req)
text_req.set('request')
lreq.grid(row=4,column=5)


'''''definition for lable and entry of Description'''
text_des = StringVar()
ldes=Label(window,textvar=text_des)
text_des.set('description')
ldes.grid(row=5,column=5)
box_des=StringVar(window,value='it')
box_des=Entry(window,textvar=box_des,validate="focusout", validatecommand=validate_des)
box_des.grid(row=5,column=6)


'''''definition for lable and entry of submit buttion'''
text_button=Button(window,text='submit',command=button_click)
text_button.grid(row=9,column=6)

window.mainloop()




# Press the green button in the gutter to run the script.
# See PyCharm help at https://www.jetbrains.com/help/pycharm/
import pandas as pd
from pandas import *
from os.path import exists

def button_click():

    '''''all inputs of textboxes in validinputs list '''
    validinputs=[validate_nam(),validate_ema(),validate_psn(),
                 validate_loc(),validate_des()]

    if False not in validinputs:
        '''converting request value number to description'''
        if var.get()==1:
            request='IT'
        else:
            request='HR'


        'coverting psno to integer '
        box_psn2=int(box_psn.get())

        '''creating dictionary with textbox values '''
        dic_of_textboxes={ 'name':[box_name.get()],
                          'psno' :[box_psn2],
                          'email':[box_ema.get()],
                          'request':[request],
                           'location':[box_loc.get()],
                          'discriptiion':[box_des.get()]
                           }

        '''displayes after successfull validation after submission'''
        tkinter.messagebox.showinfo("1st click",dic_of_textboxes)

        '''if file already exist append to excel'''
        if exists('ltts.xlsx')==True:
            df = pd.read_excel('ltts.xlsx')
            global list_name,list_psno,list_email,psno

            ''''get list of names, psno,email for validation'''
            list_name=df['name'].to_list()
            list_psno=df['psno'].to_list()
            list_email=df['email'].to_list()

            ''''conditions for not be duplicate values'''
            if box_name.get() in list_name:
                print('name not available plz give another name')
            elif box_psn2 in list_psno:
                print('psno already present ,please give another psno')
            elif box_ema.get() in list_email:
                print('email already prasent,plz provide another mailid')
            else:

                '''if all condtions ware satisfied create dataframe1 by reading excel file and append new dictionary df2 to df1 and append to excel '''
                df1 = pd.DataFrame(dic_of_textboxes)
                dff = df.append(df1, ignore_index=True)

                '''removing unnamed columns'''
                dff.drop(dff.columns[dff.columns.str.contains('unnamed', case=False)], axis=1, inplace=True)
                dff.to_excel('ltts.xlsx')
                print(dff)
                print("you have successfully registered")

        else:
            ''''if file not present create new'''
            f=open('ltts.xlsx','x')
            f.close()

            ''''append record to excel'''
            df=pandas.DataFrame(dic_of_textboxes)
            df.to_excel('ltts.xlsx')
            print(df)


'''validate name by length,not tobe number'''
def validate_nam():
    name11=box_name.get()
    if len(name11)<4:
         print('must be minimum 4')
         return False
    elif name11=='':
        print('name not be empty ,plz enter ur name')
    elif name11.isdigit():
        print('name cannot be numeric,plz give valid name')
        return False
    elif name11[0].isnumeric():
        print('cannot start with numbers')
        return False
    else:
        return True

'''validate ltts email '''
def validate_ema():
    str1=box_ema.get()
    a='@ltts.com'
    str=box_ema.get().split('@')
    if ('.' not in str[0]):
        print('its not ltts mail id which hss single dot ')
        return False
    if str1.endswith('@ltts.com') != a:
        print('mail not ends with @ltts.com,plz check missing')
        return False
    elif str1.isalpha():
        print('not in the ltts mail format which not have numbers in mail')
        return False
    elif str1.isdigit():
        print('mail should not contain digits')
        return False
    else:
        return True

'''validate psn by length,not tobe number '''
def validate_psn():
    if len((box_psn.get()))== 8:
        return True
    elif type(box_psn.get())==str:
        print('should not be string ,plz enter numbers')
        return False
    else:
        print('please enter 8 digits psn number or you mixed with string')
        return False

'''validate location from list defined below'''
def validate_loc():
    locations=['hyderabad','banglore','mysore','mumbai']
    if box_loc.get() in locations:
        return True
    else:
        print('location must be an entry in the location list')
        return False


'''validate description should not be empty and numbers'''
def validate_des():
    des=box_des.get()
    if box_des.get()!='':
        if des.isdigit():
            print('plz enter  charecters')
            return False
        else:
            return True
    else:
        print('description must not be empty')
        return False




'''''main window definition'''
window=tkinter.Tk()
window.title("Registration")
window.geometry('600x600')


'''''definition for lable and entry of name'''
text_nam = StringVar()
box_name = StringVar(window,value='rajavardhanreddy')
lable1=Label(window,textvar=text_nam)
text_nam.set('name')
lable1.grid(row=0,column=5)


'''''definition for lable and entry of email'''
text_ema = StringVar()
lemail=Label(window,textvar=text_ema)
text_ema.set('email')
lemail.grid(row=1,column=5)
box_ema=StringVar(window,value='guttikondarajavardhan.reddy@ltts.com')
box_ema=Entry(window,textvar=box_ema,validate="focusout", validatecommand=validate_ema)
box_ema.grid(row=1,column=6)


'''''definition for lable and entry of psnumber'''
text_psn = IntVar()
lpsno=Label(window,textvar=text_psn)
text_psn.set('psnumber')
lpsno.grid(row=2,column=5)
box_psn=IntVar(window,value='99005559')
box_psn=Entry(window,textvar=box_psn,validate="focusout", validatecommand=validate_psn)
box_psn.grid(row=2,column=6)


'''''definition for lable and entry of location'''
text_loc = StringVar()
lloc=Label(window,textvar=text_loc)
text_loc.set('location')
lloc.grid(row=3,column=5)
box_loc=StringVar(window,value='hyderabad')
box_loc=Entry(window,textvar=box_loc,validate="focusout", validatecommand=validate_loc)
box_loc.grid(row=3,column=6)


'''''definition for request choices output '''
def viewSelected():
    choice = var.get()
    if choice == 1:
        output = "IT"
    elif choice == 2:
        output = "HR"
    else:
        output = "Invalid selection"
    return tkinter.messagebox.showinfo('PythonGuides', f'You Selected {output}.')


'''''definition for lable and entry of Request'''
var = IntVar()
Radiobutton(window, text="IT", variable=var, value=1, command=viewSelected).grid(row=4,column=6)
Radiobutton(window, text="HR", variable=var, value=2, command=viewSelected).grid(row=4,column=7)
text_req = StringVar()
lreq=Label(window,textvar=text_req)
text_req.set('request')
lreq.grid(row=4,column=5)


'''''definition for lable and entry of Description'''
text_des = StringVar()
ldes=Label(window,textvar=text_des)
text_des.set('description')
ldes.grid(row=5,column=5)
box_des=StringVar(window,value='it')
box_des=Entry(window,textvar=box_des,validate="focusout", validatecommand=validate_des)
box_des.grid(row=5,column=6)


'''''definition for lable and entry of submit buttion'''
text_box=Entry(window, textvar=box_name,validate="focusout", validatecommand=validate_nam)
text_box.grid(row=0,column=6)
text_button=Button(window,text='submit',command=button_click)
text_button.grid(row=9,column=6)

window.mainloop()




# Press the green button in the gutter to run the script.
# See PyCharm help at https://www.jetbrains.com/help/pycharm/
