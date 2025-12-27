# 014_HackX
Air Quality Sensor with an Alarm System using software based Instrumentation Amplifier

import tkinter as tk
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
from tkinter import ttk
import time as tm
import winsound as ws

gain=100
moderate_AQI=100
high_AQI=150
veryhigh_AQI=250

root=tk.Tk()
root.title("Smoke Detector and Alarm System")
root.geometry("100x100")

root.title("Slider Example")


v1_slider = tk.Scale(root, from_=0, to=5, resolution=0.01, orient='horizontal', label="V1")
v1_slider.pack()


v2_slider = tk.Scale(root, from_=0, to=5, resolution=0.01, orient='horizontal', label="V2")
v2_slider.pack()


def show_values():
    print("V1 =", v1_slider.get())
    print("V2 =", v2_slider.get())



tk.Label(root,text="AQI").pack()
AQI_entry=tk.Entry(root)
AQI_entry.pack()

tk.Label(root,text="Threshold").pack()
Threshold_entry=tk.Entry(root)
Threshold_entry.pack()

vout_label=tk.Label(root,text="Vout=--")
vout_label.pack()

AQI_label=tk.Label(root,text="Air quality=--")
AQI_label.pack()

smoke_label=tk.Label(root,text="Smoke Status=--")
smoke_label.pack()

beep_label=tk.Label(root,text="Beep=--")
beep_label.pack()



def check_air():
    print("button pressed.Checking Air Quality")


  
    AQI=float(AQI_entry.get())
    Threshold=float(Threshold_entry.get())
    
    v1=v1_slider.get()
    v2=v2_slider.get()
    vdiff=v1-v2  
    vout=gain*(vdiff)  
    vout_label.config(text="vout: "+str(vout))
    
    if AQI > veryhigh_AQI:
        AQI_label.config(fg="red")
    elif AQI > high_AQI:
        AQI_label.config(fg="orange")
    elif AQI > moderate_AQI:
        AQI_label.config(fg="brown")
    else:
        AQI_label.config(fg="green")

    if AQI>veryhigh_AQI:
        beep=3
        quality="Hazardous"
    elif AQI>high_AQI:
        beep=2
        quality="very poor"        
    elif AQI>moderate_AQI:
        beep=1
        quality="poor"
    else:
        beep=0
        quality="normal"
    AQI_label.config(text="Air Quality:"+ quality)
    beep_label.config(text="Beep level:"+ str(beep))
    if vout>=Threshold and AQI>100:
        smoke_label.config(text="Smoke Status="+"Smoke Alert!", fg="red")
        
    else:
        smoke_label.config(text="Safe", fg="green")
    for i in range(beep):
        ws.Beep(1000,300)
        tm.sleep(0.1)
        
tk.Button(root,text="click for checking",command=check_air, bg="black", fg="white").pack()

    

root.mainloop()





