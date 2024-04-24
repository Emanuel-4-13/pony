import pip
from tkinter import *
from PIL import ImageTk, Image
import math
import sympy
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.patches import Polygon

ventana = Tk()
ventana.title("Calculadora de integrales")
ventana.geometry("480x800")
ventana.configure(bg="LightGrey")

# Carga la imagen 1
imagen1 = Image.open("C:/Users/julio/OneDrive/Imágenes/pony/pony.jpg")
imagen1 = imagen1.resize((400, 400), Image.LANCZOS)#ancha la imagen de la pony

imagenCargada1 = ImageTk.PhotoImage(imagen1)
fondo1 = Label(ventana, image=imagenCargada1)
fondo1.place(x=50, y=37)

# Carga la imagen 2
imagenCargada2 = PhotoImage(file="C:/Users/julio/Videos/integral/integral.png")
fondo2 = Label(ventana, image=imagenCargada2)
fondo2.place(x=30, y=310)

# Variables tkinter
function = StringVar()
respuesta = DoubleVar()
funcion1 = StringVar()
respuesta1 = DoubleVar()
variable = StringVar()
limitei = IntVar()
limites = IntVar()

def calcular():
    try:
        x = sympy.Symbol('x')  # Definir solo un símbolo para la integración
        FUNCIONC = function.get()
        if FUNCIONC.strip():  # Verificar si la entrada no está vacía
            calculo1 = sympy.integrate(FUNCIONC, (x, limitei.get(), limites.get()))
            respuesta.set(round(calculo1, 3))
        else:
            respuesta.set("")  # Si la entrada está vacía, establecer la respuesta en blanco
    except (sympy.SympifyError, ValueError) as e:
        print("Error al integrar:", e)
        # Puedes mostrar un mensaje de error al usuario aquí si lo deseas

def calcular2():
    try:
        x = sympy.Symbol('x')  # Definir solo un símbolo para la integración
        funcion = funcion1.get()
        if funcion.strip():  # Verificar si la entrada no está vacía
            calculo2 = sympy.integrate(funcion, x)
            respuesta1.set(calculo2)
            #Mostar resultado en la caja 6
            variable.set(calculo2)
        else:
            respuesta1.set("")  # Si la entrada está vacía, establecer la respuesta en blanco
            variable.set()
    except (sympy.SympifyError, ValueError) as e:
        print("Error al integrar:", e)
        # Puedes mostrar un mensaje de error al usuario aquí si lo deseas

def calcular3():#calcular_fraciones parciales
    try:
        x = sympy.Symbol('x')
        FUNCIONC = function.get()
        if FUNCIONC.strip():
            fracciones_parciales = sympy.apart(FUNCIONC, x)  # Descomponer en fracciones parciales
            integrales = [sympy.integrate(frac, x) for frac in fracciones_parciales]  # Integrar cada fracción parcial
            resultado = sum(integrales)  # Sumar las integrales de las fracciones parciales
            respuesta.set(resultado)
        else:
            respuesta.set("") 
    except (sympy.SympifyError, ValueError) as e:
        print("Error al integrar:", e)
        


def grafica():
    def func(x):
        return x**2 + 2*x
    
    a, b = limitei.get(), limites.get()
    x = np.linspace(0, 10)
    y = func(x)
    
    fig, ax = plt.subplots()
    plt.plot(x, y, "r", linewidth=1)
    plt.ylim(0)
    
    ix = np.linspace(a, b)
    iy = func(ix)
    verts = [(a, 0)] + list(zip(ix, iy)) + [(b, 0)]
    poly = Polygon(verts, facecolor='0.9', edgecolor='0.5')
    ax.add_patch(poly)
    
    plt.text(0.6 * (a + b), 6, r"$\int_a^b f(x)\mathrm{d}x$",
             horizontalalignment='center', fontsize=10)
    
    plt.figtext(0.9, 0.05, '$x$')
    plt.figtext(0.1, 0.9, '$y$')
    
    ax.spines['right'].set_visible(False)
    ax.spines['top'].set_visible(False)
    ax.xaxis.set_ticks_position('bottom')
    
    x = np.arange(-15, 15, 0.1)
    plt.plot(x, [func(i) for i in x])
    
    plt.axhline(0, color="black")
    plt.axhline(0, color="black")
    
    plt.xlim(-10, 10)
    plt.ylim(-10, 10)
    
    plt.savefig("output.png")
    plt.grid(True)
    plt.show()

def grafica2():
    def func(x):
        return np.sin(x) * np.tan(x)
    
    x = np.linspace(0, 10)
    y = func(x)
    
    fig, ax = plt.subplots()
    plt.plot(x, y, "r", linewidth=1)
    plt.ylim(0)
    
    plt.figtext(0.9, 0.02, '$x$')
    plt.figtext(0.1, 0.9, '$y$')
    
    ax.spines['right'].set_visible(False)
    ax.spines['top'].set_visible(False)
    ax.xaxis.set_ticks_position('bottom')
    
    x = np.arange(-15, 15, 0.1)
    plt.plot(x, [func(i) for i in x])
    
    plt.axhline(0, color="black")
    plt.axvline(0, color="black")
    
    plt.xlim(-10, 10)
    plt.ylim(-10, 10)
    
    plt.savefig("output.png")
    plt.grid(True)
    plt.show()

caja1 = Entry(ventana, textvariable=function, font=("Arial Bold", 13), width=30, relief=RIDGE, borderwidth=4, insertwidth=3)
caja1.place(x=170, y=200)#mueve la placa de integral

caja2 = Entry(ventana, textvariable=respuesta, font=("Arial Bold", 13), width=30, relief=RIDGE, borderwidth=4, insertwidth=3)
caja2.place(x=169, y=110)#mueve la placa 0.0

caja3 = Entry(ventana, textvariable=limitei, font=("Arial Bold", 13), width=3, relief=RIDGE, borderwidth=4, insertwidth=3)
caja3.place(x=70, y=150)#mueve el primer bloque 0 

caja4 = Entry(ventana, textvariable=limites, font=("Arial Bold", 13), width=3, relief=RIDGE, borderwidth=4, insertwidth=3)
caja4.place(x=70, y=185)#mueve el segundo bloque 0

caja5 = Entry(ventana, textvariable=funcion1, font=("Arial Bold", 13), width=30, relief=RIDGE, borderwidth=4, insertwidth=3)
caja5.place(x=170, y=73)# mueve el tercer bloque 

caja6 = Entry(ventana, textvariable=variable, font=("Arial Bold", 14), width=10, relief=RIDGE, borderwidth=3, insertwidth=3)
caja6.place(x=150, y=600)#mueve el cuarto bloque

caja7 = Entry(ventana, textvariable=respuesta, font=("Arial Bold", 13), width=30, relief=RIDGE, borderwidth=4, insertwidth=3)
caja7.place(x=169, y=430)#posiciona la caja de la respuesta de las fracciones parciales

Boton_fp = Button(ventana, text="Calcular con Fracciones Parciales", bg="DarkGrey", font=("Arial Bold", 10), width=30, height=1, command=calcular3)
Boton_fp.place(x=100, y=380) #posiciona el boton para calcular con fracciones parciales

etiqueta1 = Label(ventana, text="Area", font=("Nueva operacion", 15), relief=RIDGE)
etiqueta1.place(x=30, y=600)#mueve el bloque  area

etiqueta2 = Label(ventana, text="Integrales por partes", font=("Nueva operacion", 20), relief=RIDGE)
etiqueta2.place(x=100, y=0)# mueve el bloque integrales por partes

etiqueta3 = Label(ventana, text="Integrales Fracciones Parciales", font=("Nueva operacion", 20), relief=RIDGE)
etiqueta3.place(x=60, y=320)#mueve el bloque integrales fraciones 

etiqueta4 = Label(ventana, text="Integrales Sustitucion Trigonometrica", font=("Nueva operacion", 15), relief=RIDGE)
etiqueta4.place(x=40, y=500)#mueve el bloue sustitucion trigonometrica

etiqueta5 = Label(ventana, text="Integral", font=("Nueva operacion", 12), relief=RIDGE)
etiqueta5.place(x=70, y=76)#mueve el bloque integral

Boton1 = Button(ventana, text="Calcular4", bg="DarkGrey", font=("Arial Bold", 10), width=8, height=1, command=calcular)
Boton1.place(x=370, y=700)# calcular 4

Boton2 = Button(ventana, text="Calcula1", bg="DarkGrey", font=("Arial Bold", 10), width=8, height=1, command=calcular2)
Boton2.place(x=30, y=700)#mueve el bloque 1

Boton3 = Button(ventana, text="Mostrar grafica ", bg="DarkGrey", font=("Arial Bold", 10), width=12, height=1, command=grafica)
Boton3.place(x=120, y=700) #mueve la primera grafica

Boton4 = Button(ventana, text="Mostrar grafica", bg="DarkGrey", font=("Arial Bold", 10), width=12, height=1, command=grafica2)
Boton4.place(x=250, y=700)#mueve la segunda grafica

ventana.mainloop()




