import sqlite3
from tkcalendar import DateEntry
from datetime import datetime
from tkinter import *
from tkinter import ttk, messagebox
from PIL import Image, ImageTk
import os

class HotelManagementSystem:
    def __init__(self, root):
        self.root = root
        self.root.title("Hotel Management System")
        self.root.geometry("1550x800+0+0")

        # Fondo
        img1 = Image.open(r"/Users/danielgallegolazaro/Downloads/fondo.png")
        img1 = img1.resize((1550, 140), Image.Resampling.LANCZOS)
        self.photoimg1 = ImageTk.PhotoImage(img1)
        Label(self.root, image=self.photoimg1, bd=4, relief=RIDGE).place(x=0, y=0, width=1550, height=140)

        # Logo
        img2 = Image.open(r"/Users/danielgallegolazaro/Downloads/Logo.png")
        img2 = img2.resize((230, 140), Image.Resampling.LANCZOS)
        self.photoimg2 = ImageTk.PhotoImage(img2)
        Label(self.root, image=self.photoimg2, bd=4, relief=RIDGE).place(x=0, y=0, width=230, height=140)

        # Título
        Label(self.root, text="HOTEL MANAGEMENT SYSTEM", font=("times new roman", 40, "bold"),
              bg="black", fg="gold", bd=4, relief=RIDGE).place(x=0, y=140, width=1550, height=50)

        # Cuadro principal
        main_frame = Frame(self.root, bd=4, relief=RIDGE)
        main_frame.place(x=0, y=190, width=1550, height=620)

        # Menu principal
        Label(main_frame, text="MENU", font=("times new roman", 20, "bold"), bg="black", fg="gold",
              bd=4, relief=RIDGE).place(x=0, y=0, width=230)

        # Botones
        btn_frame = Frame(main_frame, bd=4, relief=RIDGE)
        btn_frame.place(x=0, y=35, width=228, height=190)
        Button(btn_frame, text="CLIENTE", command=self.cust_details, width=22, font=("times new roman", 14, "bold"),
               bg="black", fg="gold", bd=0).grid(row=0, column=0, pady=1)
        Button(btn_frame, text="HABITACIÓN", command=self.room_details, width=22,
               font=("times new roman", 14, "bold"), bg="black", fg="gold", bd=0).grid(row=1, column=0, pady=1)
        Button(btn_frame, text="DETALLES", command=self.details_view, width=22,
               font=("times new roman", 14, "bold"), bg="black", fg="gold", bd=0).grid(row=2, column=0, pady=1)

        Button(btn_frame, text="INCIDENCIAS", command=self.incidencias_view, width=22,
               font=("times new roman", 14, "bold"),
               bg="black", fg="gold", bd=0).grid(row=3, column=0, pady=1)

        Button(btn_frame, text="SALIR", command=self.root.quit, width=22, font=("times new roman", 14, "bold"),
               bg="black", fg="gold", bd=0).grid(row=4, column=0, pady=1)

        # Área derecha
        self.right_frame = Frame(main_frame, bd=4, relief=RIDGE)
        self.right_frame.place(x=225, y=0, width=1310, height=590)

        # Logos inferiores
        img_logo = Image.open(r"/Users/danielgallegolazaro/Downloads/Logo.png")
        for y in [225, 420]:
            img = img_logo.resize((230, 210 if y == 225 else 190), Image.Resampling.LANCZOS)
            photo = ImageTk.PhotoImage(img)
            Label(main_frame, image=photo, bd=4, relief=RIDGE).place(x=0, y=y, width=230, height=210 if y == 225 else 190)
            setattr(self, f"photoimg_y{y}", photo)

    def cust_details(self):
        for widget in self.right_frame.winfo_children():
            widget.destroy()
        self.app = Cust_Win(self.right_frame)

    def room_details(self):
        for widget in self.right_frame.winfo_children():
            widget.destroy()
        self.app = Roombooking(self.right_frame)

    def details_view(self):
        for widget in self.right_frame.winfo_children():
            widget.destroy()
        self.app = Detalles(self.right_frame)

    def incidencias_view(self):
        for widget in self.right_frame.winfo_children():
            widget.destroy()
        self.app = Incidencias(self.right_frame)


class Cust_Win:
    def __init__(self, parent_frame):
        self.root = parent_frame
        self.conectar_bd()
        self.crear_tabla()

        Label(self.root, text="Añadir datos del cliente", font=("times new roman", 18, "bold"),
              bg="black", fg="gold").place(x=0, y=0, width=1295, height=50)

        labelframeleft = LabelFrame(self.root, bd=2, relief=RIDGE, text="Datos del Cliente",
                                    font=("Arial", 12, "bold"), padx=2)
        labelframeleft.place(x=5, y=60, width=580, height=500)

        fields = [
            ("Customer Ref", ttk.Entry),
            ("Customer Name", ttk.Entry),
            ("Mother Name", ttk.Entry),
            ("Gender", ttk.Combobox),
            ("Código Postal", ttk.Entry),
            ("Móvil", ttk.Entry),
            ("Email", ttk.Entry),
            ("Nacionalidad", ttk.Entry),
            ("Tipo de Documento", ttk.Combobox),
            ("Número de Documento", ttk.Entry),
            ("Dirección", ttk.Entry)
        ]

        self.campos = {}
        for idx, (label_text, widget_type) in enumerate(fields):
            Label(labelframeleft, text=label_text, font=("Arial", 12, "bold"), padx=2, pady=6).grid(row=idx, column=0, sticky="w")
            if widget_type == ttk.Combobox:
                widget = widget_type(labelframeleft, font=("Arial", 13, "bold"), width=27, state="readonly")
                widget["values"] = ("DNI", "Pasaporte", "NIE") if "Documento" in label_text else ("Male", "Female", "Other")
                widget.current(0)
            else:
                widget = widget_type(labelframeleft, font=("Arial", 13, "bold"), width=29)
            widget.grid(row=idx, column=1)
            self.campos[label_text] = widget

        btn_frame = Frame(labelframeleft, bg="black")
        btn_frame.grid(row=len(fields), column=0, columnspan=2, pady=10)
        Button(btn_frame, text="AÑADIR", font=("Arial", 13), bg="black", fg="gold", width=12, command=self.insertar_cliente).grid(row=0, column=0, padx=5)
        Button(btn_frame, text="ACTUALIZAR", font=("Arial", 13), bg="black", fg="gold", width=12, command=self.actualizar_cliente).grid(row=0, column=1, padx=5)
        Button(btn_frame, text="BORRAR", font=("Arial", 13), bg="black", fg="gold", width=12, command=self.borrar_cliente).grid(row=0, column=2, padx=5)
        Button(btn_frame, text="RESET", font=("Arial", 13), bg="black", fg="gold", width=12, command=self.reset_campos).grid(row=0, column=3, padx=5)

        self.crear_interfaz_tabla()
        self.mostrar_todos()

    def crear_interfaz_tabla(self):
        Table_Frame = LabelFrame(self.root, bd=2, relief=RIDGE, text="Ver detalles y sistema de búsqueda",
                                 font=("arial", 12, "bold"))
        Table_Frame.place(x=600, y=60, width=680, height=500)

        lblSearchBy = Label(Table_Frame, font=("arial", 12, "bold"), text="Buscar por:", bg="red", fg="white")
        lblSearchBy.grid(row=0, column=0, sticky=W, padx=2)

        self.combo_Search = ttk.Combobox(Table_Frame, font=("arial", 12, "bold"), width=24, state="readonly")
        self.combo_Search["value"] = ("movil", "ref")
        self.combo_Search.current(0)
        self.combo_Search.grid(row=0, column=1, padx=2)

        self.txtSearch = ttk.Entry(Table_Frame, font=("arial", 13, "bold"), width=24)
        self.txtSearch.grid(row=0, column=2, padx=2)

        btnSearch = Button(Table_Frame, text="Buscar", font=("arial", 11, "bold"), bg="black", fg="gold", width=8, command=self.buscar_cliente)
        btnSearch.grid(row=0, column=3, padx=1)

        btnShowAll = Button(Table_Frame, text="Mostrar", font=("arial", 11, "bold"), bg="black", fg="gold", width=8, command=self.mostrar_todos)
        btnShowAll.grid(row=0, column=4, padx=1)

        details_table = Frame(Table_Frame, bd=2, relief=RIDGE)
        details_table.place(x=0, y=40, width=650, height=400)

        scroll_x = ttk.Scrollbar(details_table, orient=HORIZONTAL)
        scroll_y = ttk.Scrollbar(details_table, orient=VERTICAL)

        self.Cust_Details_Table = ttk.Treeview(
            details_table,
            columns=("ref", "nombre", "madre", "genero", "cod_postal", "movil", "email", "nacionalidad", "tipo_doc", "num_doc", "direccion"),
            xscrollcommand=scroll_x.set,
            yscrollcommand=scroll_y.set
        )

        scroll_x.pack(side=BOTTOM, fill=X)
        scroll_y.pack(side=RIGHT, fill=Y)

        scroll_x.config(command=self.Cust_Details_Table.xview)
        scroll_y.config(command=self.Cust_Details_Table.yview)

        self.Cust_Details_Table.pack(fill=BOTH, expand=1)

        headers = {
            "ref": "Referencia",
            "nombre": "Nombre",
            "madre": "Nombre madre",
            "genero": "Género",
            "cod_postal": "Código postal",
            "movil": "Móvil",
            "email": "Correo electrónico",
            "nacionalidad": "Nacionalidad",
            "tipo_doc": "Tipo de documento",
            "num_doc": "Nº documento",
            "direccion": "Dirección"
        }

        for col, texto in headers.items():
            self.Cust_Details_Table.heading(col, text=texto)
            self.Cust_Details_Table.column(col, width=120)

        self.Cust_Details_Table["show"] = "headings"
        self.Cust_Details_Table.bind("<Double-1>", self.seleccionar_fila)

    def seleccionar_fila(self, event):
        item = self.Cust_Details_Table.focus()
        valores = self.Cust_Details_Table.item(item, "values")
        if valores:
            for (clave, campo), valor in zip(self.campos.items(), valores):
                if isinstance(campo, ttk.Combobox):
                    try:
                        campo.set(valor)
                    except:
                        pass
                else:
                    campo.delete(0, END)
                    campo.insert(0, valor)

    def buscar_cliente(self):
        campo = self.combo_Search.get()
        valor = self.txtSearch.get()
        self.cursor.execute(f"SELECT * FROM clientes WHERE {campo} LIKE ?", (f"%{valor}%",))
        filas = self.cursor.fetchall()
        self.Cust_Details_Table.delete(*self.Cust_Details_Table.get_children())
        for fila in filas:
            self.Cust_Details_Table.insert("", END, values=fila)

    def mostrar_todos(self):
        self.cursor.execute("SELECT * FROM clientes")
        filas = self.cursor.fetchall()
        self.Cust_Details_Table.delete(*self.Cust_Details_Table.get_children())
        for fila in filas:
            self.Cust_Details_Table.insert("", END, values=fila)

    def conectar_bd(self):
        self.conn = sqlite3.connect("clientes.db")
        self.cursor = self.conn.cursor()

    def crear_tabla(self):
        self.cursor.execute("""
            CREATE TABLE IF NOT EXISTS clientes (
                ref TEXT PRIMARY KEY,
                nombre TEXT,
                madre TEXT,
                genero TEXT,
                cod_postal TEXT,
                movil TEXT,
                email TEXT,
                nacionalidad TEXT,
                tipo_doc TEXT,
                num_doc TEXT,
                direccion TEXT
            )
        """)
        self.conn.commit()



    def insertar_cliente(self):
        datos = [campo.get() for campo in self.campos.values()]

        email = datos[6]
        movil = datos[5]
        tipo_doc = self.campos["Tipo de Documento"].get()
        num_doc = datos[9]

        import re
        if not re.match(r"[^@]+@[^@]+\.[^@]+", email):
            messagebox.showerror("Error", "Correo electrónico no válido")
            return

        if not movil.isdigit() or not (9 <= len(movil) <= 15):
            messagebox.showerror("Error", "Número de móvil no válido")
            return

        if tipo_doc == "DNI" and not re.match(r"^\d{8}[A-Z]$", num_doc):
            messagebox.showerror("Error", "DNI no válido (formato: 8 números + letra mayúscula)")
            return

        if tipo_doc in ("Pasaporte", "NIE") and not re.match(r"^[A-Z]\d{7}[A-Z]$", num_doc):
            messagebox.showerror("Error", f"{tipo_doc} no válido (formato: letra + 7 números + letra)")
            return

        if not datos[0]:
            messagebox.showerror("Error", "Referencia obligatoria")
            return
        try:
            self.cursor.execute("INSERT INTO clientes VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)", datos)
            self.conn.commit()
            messagebox.showinfo("Éxito", "Cliente añadido correctamente")
            self.mostrar_todos()
        except sqlite3.IntegrityError:
            messagebox.showwarning("Aviso", "El cliente ya existe")

    def actualizar_cliente(self):
        ref = self.campos["Customer Ref"].get()
        if not ref:
            messagebox.showerror("Error", "Referencia obligatoria para actualizar")
            return
        self.cursor.execute("SELECT * FROM clientes WHERE ref = ?", (ref,))
        if not self.cursor.fetchone():
            messagebox.showwarning("Aviso", "Cliente no encontrado")
            return
        datos = [campo.get() for campo in self.campos.values()][1:] + [ref]
        self.cursor.execute("""
            UPDATE clientes SET
                nombre=?, madre=?, genero=?, cod_postal=?, movil=?, email=?, nacionalidad=?, tipo_doc=?, num_doc=?, direccion=?
            WHERE ref=?
        """, datos)
        self.conn.commit()
        messagebox.showinfo("Actualizado", "Datos actualizados correctamente")
        self.mostrar_todos()

    def borrar_cliente(self):
        ref = self.campos["Customer Ref"].get()
        if not ref:
            messagebox.showerror("Error", "Referencia obligatoria para borrar")
            return
        if messagebox.askyesno("Confirmar", "¿Estás seguro de que quieres borrar este cliente?"):
            self.cursor.execute("DELETE FROM clientes WHERE ref = ?", (ref,))
            self.conn.commit()
            messagebox.showinfo("Borrado", "Cliente eliminado correctamente")
            self.reset_campos()
            self.mostrar_todos()

    def reset_campos(self):
        for campo in self.campos.values():
            if isinstance(campo, ttk.Combobox):
                campo.current(0)
            else:
                campo.delete(0, END)


class Roombooking:
    def __init__(self, root):
        self.root = root
        self.conectar_bd()
        self.crear_tabla()

        lbl_title = Label(self.root, text="DETALLES DE RESERVA DE HABITACIÓN",
                          font=("times new roman", 18, "bold"), bg="black", fg="gold")
        lbl_title.place(x=0, y=0, width=1295, height=50)

        labelframeleft = LabelFrame(self.root, bd=2, relief=RIDGE, text="Datos de la Reserva",
                                    font=("arial", 12, "bold"), padx=2)
        labelframeleft.place(x=5, y=50, width=425, height=520)

        # Campos
        self.datos = {}
        labels = [
            ("Contacto del Cliente", "entry"),
            ("Nº de Personas", "entry"),
            ("Fecha de Entrada", "date"),
            ("Fecha de Salida", "date"),
            ("Habitación", "combo", ["Estándar", "Familiar", "Suite"]),
            ("Comida", "combo", ["Desayuno", "Media pensión", "Pensión completa"]),
            ("Nº de Días", "entry"),
            ("Impuesto Pagado", "entry"),
            ("Subtotal", "entry"),
            ("Costo Total", "entry"),
            ("Ver Habitaciones", "button")
        ]



        for idx, (label, tipo, *extra) in enumerate(labels):
            Label(labelframeleft, text=label + ":", font=("arial", 11, "bold")).grid(row=idx, column=0, sticky=W,
                                                                                     padx=5, pady=5)

            if tipo == "entry":
                entry = Entry(labelframeleft, width=25, font=("arial", 11))
            elif tipo == "combo":
                entry = ttk.Combobox(labelframeleft, values=extra[0], state="readonly", font=("arial", 11), width=23)
                entry.current(0)
            elif tipo == "date":
                entry = DateEntry(labelframeleft, width=22, font=("arial", 11), date_pattern='dd-mm-yyyy',
                                  showweeknumbers=False)
                entry.bind("<<DateEntrySelected>>", lambda e: self.calcular_dias())
            elif tipo == "button":
                entry = Button(labelframeleft, text="Ver Habitaciones", font=("arial", 10, "bold"),
                               bg="black", fg="gold", width=22, command=self.mostrar_plano_habitaciones)
            else:
                entry = Entry(labelframeleft, width=25, font=("arial", 11))

            entry.grid(row=idx, column=1, padx=5)

            if tipo != "button":
                self.datos[label] = entry

        # ✅ Ahora sí puedes asociar eventos, porque self.datos ya existe:
        self.datos["Habitación"].bind("<<ComboboxSelected>>", lambda e: self.calcular_precio_total())
        self.datos["Comida"].bind("<<ComboboxSelected>>", lambda e: self.calcular_precio_total())
        self.datos["Nº de Personas"].bind("<FocusOut>", lambda e: self.calcular_precio_total())

        # Botón buscar
        Button(labelframeleft, text="Buscar Datos", font=("arial", 10, "bold"), bg="black", fg="gold", width=12,
               command=self.buscar_datos_cliente).grid(row=0, column=2, padx=5)

        # Botones CRUD
        botones = [
            ("Añadir", self.insertar_reserva),
            ("Actualizar", self.actualizar_reserva),
            ("Eliminar", self.eliminar_reserva),
            ("Reset", self.reset_campos)
        ]
        for i, (texto, cmd) in enumerate(botones):
            Button(labelframeleft, text=texto, font=("arial", 11, "bold"), bg="black", fg="gold", width=10,
                   command=cmd).place(x=10 + i * 100, y=460)

        # Tabla a la derecha
        frame_tabla = Frame(self.root, bd=2, relief=RIDGE)
        frame_tabla.place(x=440, y=150, width=830, height=405)

        scroll_x = Scrollbar(frame_tabla, orient=HORIZONTAL)
        scroll_y = Scrollbar(frame_tabla, orient=VERTICAL)

        self.reserva_tabla = ttk.Treeview(frame_tabla,
                                          columns=("contacto", "personas", "checkin", "checkout", "habitacion",
                                                   "comida", "dias", "impuesto", "subtotal", "total"),

                                          xscrollcommand=scroll_x.set,
                                          yscrollcommand=scroll_y.set)

        scroll_x.pack(side=BOTTOM, fill=X)
        scroll_y.pack(side=RIGHT, fill=Y)
        scroll_x.config(command=self.reserva_tabla.xview)
        scroll_y.config(command=self.reserva_tabla.yview)
        self.reserva_tabla.pack(fill=BOTH, expand=1)

        for col in self.reserva_tabla["columns"]:
            self.reserva_tabla.heading(col, text=col.capitalize())
            self.reserva_tabla.column(col, width=100)

        self.reserva_tabla["show"] = "headings"
        self.reserva_tabla.bind("<Double-1>", self.seleccionar_fila)

        self.mostrar_reservas()

    def conectar_bd(self):
        self.conn = sqlite3.connect("reservas.db")
        self.cursor = self.conn.cursor()



    def crear_tabla(self):
        self.cursor.execute("DROP TABLE IF EXISTS reservas")  # <-- Elimina la tabla si existe

        self.cursor.execute("""
            CREATE TABLE reservas (
                contacto TEXT PRIMARY KEY,
                personas TEXT,
                checkin TEXT,
                checkout TEXT,
                habitacion TEXT,
                comida TEXT,
                dias TEXT,
                impuesto TEXT,
                subtotal TEXT,
                total TEXT
            )
        """)
        self.conn.commit()

    def on_fecha_cambiada(self, event=None):
        self.calcular_dias()

    def validar_fechas(self):
        try:
            entrada = self.datos["Fecha de Entrada"].get_date()
            salida = self.datos["Fecha de Salida"].get_date()
            if entrada > salida:
                messagebox.showerror("Error", "La fecha de entrada debe ser anterior o igual a la de salida.")
                return False
        except Exception as e:
            messagebox.showerror("Error de fecha", f"Ocurrió un error al validar fechas: {e}")
            return False
        return True

    def insertar_reserva(self):
        if not self.validar_fechas():
            return

        orden_campos = [
            "Contacto del Cliente",
            "Nº de Personas",
            "Fecha de Entrada",
            "Fecha de Salida",
            "Habitación",  # será usada como "habitacion" en la BD
            "Comida",
            "Nº de Días",
            "Impuesto Pagado",
            "Subtotal",
            "Costo Total"
        ]

        try:
            datos = [self.datos[campo].get() for campo in orden_campos]
            habitacion = self.datos["Habitación"].get()
            if not habitacion or not habitacion.split()[0].isdigit():
                messagebox.showerror("Error", "Debes seleccionar una habitación válida del plano (ej: 101 Estándar).")
                return

        except KeyError as e:
            messagebox.showerror("Error", f"Falta el campo: {e}")
            return

        if not datos[0]:
            messagebox.showerror("Error", "El contacto del cliente es obligatorio.")
            return

        try:
            self.cursor.execute("""
                INSERT INTO reservas 
                (contacto, personas, checkin, checkout, habitacion, comida, dias, impuesto, subtotal, total)
                VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
            """, datos)

            self.conn.commit()
            messagebox.showinfo("Éxito", "Reserva añadida correctamente.")
            self.mostrar_reservas()
        except sqlite3.IntegrityError:
            messagebox.showwarning("Aviso", "Ya existe una reserva para este contacto.")

    def mostrar_reservas(self):
        self.cursor.execute("SELECT * FROM reservas")
        filas = self.cursor.fetchall()
        self.reserva_tabla.delete(*self.reserva_tabla.get_children())
        for fila in filas:
            self.reserva_tabla.insert("", END, values=fila)

    def seleccionar_fila(self, event):
        item = self.reserva_tabla.focus()
        valores = self.reserva_tabla.item(item, "values")

        campos_ordenados = [
            "Contacto del Cliente",
            "Nº de Personas",
            "Fecha de Entrada",
            "Fecha de Salida",
            "Habitación",
            "Comida",
            "Nº de Días",
            "Impuesto Pagado",
            "Subtotal",
            "Costo Total"
        ]

        if valores and len(valores) == len(campos_ordenados):
            for campo, valor in zip(campos_ordenados, valores):
                widget = self.datos.get(campo)
                if widget:
                    if isinstance(widget, DateEntry):
                        try:
                            fecha_dt = datetime.strptime(valor, "%d-%m-%Y").date()
                            widget.set_date(fecha_dt)
                        except Exception as e:
                            messagebox.showwarning("Advertencia", f"No se pudo cargar la fecha {valor}")
                    elif isinstance(widget, ttk.Combobox):
                        widget.set(valor)
                    else:
                        widget.delete(0, END)
                        widget.insert(0, valor)

    def actualizar_reserva(self):
        orden_campos = [
            "Contacto del Cliente",
            "Nº de Personas",
            "Fecha de Entrada",
            "Fecha de Salida",
            "Habitación",
            "Comida",
            "Nº de Días",
            "Impuesto Pagado",
            "Subtotal",
            "Costo Total"
        ]

        try:
            datos = [self.datos[campo].get() for campo in orden_campos]
        except KeyError as e:
            messagebox.showerror("Error", f"Falta el campo: {e}")
            return

        contacto = datos[0]

        self.cursor.execute("SELECT * FROM reservas WHERE contacto = ?", (contacto,))
        if not self.cursor.fetchone():
            messagebox.showerror("Error", "Reserva no encontrada.")
            return

        try:
            self.cursor.execute("""
                UPDATE reservas SET
                    personas=?, checkin=?, checkout=?, habitacion=?, comida=?, dias=?, impuesto=?, subtotal=?, total=?
                WHERE contacto=?
            """, datos[1:] + [contacto])

            self.conn.commit()
            self.mostrar_reservas()
            messagebox.showinfo("Éxito", "Reserva actualizada correctamente.")
        except Exception as e:
            messagebox.showerror("Error", f"No se pudo actualizar la reserva:\n{e}")

    def eliminar_reserva(self):
        contacto = self.datos["Contacto del Cliente"].get()
        if messagebox.askyesno("Confirmar", "¿Deseas eliminar esta reserva?"):
            self.cursor.execute("DELETE FROM reservas WHERE contacto=?", (contacto,))
            self.conn.commit()
            self.mostrar_reservas()
            self.reset_campos()

    def reset_campos(self):
        for key, campo in self.datos.items():
            if isinstance(campo, DateEntry):
                campo.set_date(datetime.today().date())
            elif isinstance(campo, ttk.Combobox):
                campo.current(0)
            else:
                campo.delete(0, END)

    def buscar_datos_cliente(self):
        contacto = self.datos["Contacto del Cliente"].get()

        if not contacto:
            messagebox.showwarning("Aviso", "Por favor, introduce un número de contacto.")
            return

        try:
            conn = sqlite3.connect("clientes.db")
            cursor = conn.cursor()
            cursor.execute("SELECT * FROM clientes WHERE movil=?", (contacto,))
            cliente = cursor.fetchone()
            conn.close()

            if cliente:
                messagebox.showinfo("Cliente encontrado", "✅ Cliente correcto.")
            else:
                if messagebox.askyesno("Cliente no encontrado",
                                       "❌ Cliente no registrado. ¿Deseas ir a la pestaña de clientes para añadirlo?"):
                    self.root.event_generate("<<CambiarAPestanaClientes>>")
        except Exception as e:
            messagebox.showerror("Error", f"No se pudo acceder a los datos del cliente: {e}")

    def calcular_dias(self):
        try:
            entrada = self.datos["Fecha de Entrada"].get_date()
            salida = self.datos["Fecha de Salida"].get_date()
            dias = (salida - entrada).days
            if dias < 0:
                dias = 0
            self.datos["Nº de Días"].delete(0, END)
            self.datos["Nº de Días"].insert(0, str(dias))
            self.calcular_precio_total()
        except Exception as e:
            print("Error al calcular días:", e)

    def mostrar_plano_habitaciones(self):
        ventana = Toplevel(self.root)
        ventana.title("Plano de Habitaciones")
        ventana.geometry("700x400")

        try:
            entrada = self.datos["Fecha de Entrada"].get_date().strftime("%Y-%m-%d")
            salida = self.datos["Fecha de Salida"].get_date().strftime("%Y-%m-%d")
        except Exception:
            messagebox.showerror("Error", "Selecciona fechas válidas antes de ver habitaciones.")
            ventana.destroy()
            return

        for planta in range(3):  # Planta 1 = 100, Planta 2 = 200, etc.
            Label(ventana, text=f"Planta {planta + 1}", font=("arial", 11, "bold")).grid(row=planta * 5, column=0,
                                                                                         pady=5)

            for i in range(10):  # Habitaciones por planta
                numero = (planta + 1) * 100 + i + 1  # 101, 102, ..., 110, 201, ...
                tipo = self.obtener_tipo_habitacion(i + 1)  # 1-5 estándar, etc.

                habitacion_id = f"{numero} {tipo}"
                disponible = self.habitacion_disponible(habitacion_id, entrada, salida)
                color = "green" if disponible else "red"

                btn = Button(
                    ventana,
                    text=habitacion_id,
                    bg=color,
                    width=10,
                    height=3,
                    command=lambda t=habitacion_id: self.datos["Habitación"].set(t)
                )
                btn.grid(row=planta * 5 + (i // 5) + 1, column=i % 5 + 1, padx=5, pady=5)

    def obtener_tipo_habitacion(self, numero):
        # 5 estándar (1-5), 4 familiar (6-9), 1 suite (10) por planta
        pos = (numero - 1) % 10
        if pos < 5:
            return "Estándar"
        elif pos < 9:
            return "Familiar"
        else:
            return "Suite"

    def habitacion_disponible(self, habitacion_id, entrada, salida):
        try:
            self.cursor.execute("""
                SELECT 1 FROM reservas 
                WHERE habitacion = ? 
                AND (
                    date(checkin) <= date(?) AND date(checkout) >= date(?)
                )
            """, (habitacion_id, salida, entrada))
            resultado = self.cursor.fetchone()
            return resultado is None  # Si no hay reservas, está libre
        except Exception as e:
            print("Error al comprobar disponibilidad:", e)
            return False

    def calcular_precio_total(self):
        try:
            dias_str = self.datos["Nº de Días"].get()
            personas_str = self.datos["Nº de Personas"].get()

            if not dias_str.isdigit() or not personas_str.isdigit():
                return  # Campos vacíos o inválidos

            dias = int(dias_str)
            personas = int(personas_str)

            precios_habitacion = {
                "Estándar": 80,
                "Familiar": 120,
                "Suite": 180
            }

            precios_comida = {
                "Desayuno": 8,
                "Media pensión": 18,
                "Pensión completa": 30
            }

            habitacion = self.datos["Habitación"].get()
            comida = self.datos["Comida"].get()

            # ✅ Extrae solo el tipo: "Estándar" desde "101 Estándar"
            tipo_habitacion = habitacion.split()[-1]
            precio_habitacion = precios_habitacion.get(tipo_habitacion, 0)
            precio_comida = precios_comida.get(comida, 0)

            subtotal = (precio_habitacion * dias) + (precio_comida * personas * dias)

            impuesto = subtotal * 0.10
            total = subtotal + impuesto

            self.datos["Subtotal"].delete(0, END)
            self.datos["Subtotal"].insert(0, f"{subtotal:.2f}")

            self.datos["Impuesto Pagado"].delete(0, END)
            self.datos["Impuesto Pagado"].insert(0, f"{impuesto:.2f}")

            self.datos["Costo Total"].delete(0, END)
            self.datos["Costo Total"].insert(0, f"{total:.2f}")

        except Exception as e:
            print("Error al calcular precio total:", e)


class Detalles:
    def __init__(self, parent_frame):
        self.root = parent_frame
        self.root.configure(bg="white")

        self.tipo_var = StringVar()
        self.numero_var = StringVar()
        self.piso_var = StringVar()

        Label(self.root, text="New Rooms Add", font=("times new roman", 20, "bold"), fg="green", bg="white").place(x=5, y=0)

        # Imagen superior
        img = Image.open(r"/Users/danielgallegolazaro/Downloads/fondo.png")  # Cambia esta ruta
        img = img.resize((500, 120), Image.LANCZOS)
        self.photoimg = ImageTk.PhotoImage(img)
        Label(self.root, image=self.photoimg, bd=2, relief=RIDGE).place(x=5, y=30, width=500, height=120)

        # Frame izquierdo (formulario)
        frame_form = LabelFrame(self.root, text="Room Information", font=("arial", 12, "bold"), bg="white", bd=2, relief=RIDGE)
        frame_form.place(x=5, y=150, width=500, height=320)

        # Formulario
        Label(frame_form, text="Planta", font=("arial", 11, "bold"), bg="white").grid(row=0, column=0, padx=10, pady=10,
                                                                                      sticky=W)
        self.combo_piso = ttk.Combobox(frame_form, textvariable=self.piso_var, font=("arial", 11), width=28,
                                       state="readonly")
        self.combo_piso["values"] = ("1ª", "2ª", "3ª")
        self.combo_piso.current(0)
        self.combo_piso.grid(row=0, column=1)

        Label(frame_form, text="Nº habitación", font=("arial", 11, "bold"), bg="white").grid(row=1, column=0, padx=10,
                                                                                             pady=10, sticky=W)
        Entry(frame_form, textvariable=self.numero_var, font=("arial", 11), width=30).grid(row=1, column=1)

        Label(frame_form, text="Habitación", font=("arial", 11, "bold"), bg="white").grid(row=2, column=0, padx=10,
                                                                                          pady=10, sticky=W)
        self.combo_tipo = ttk.Combobox(frame_form, textvariable=self.tipo_var, font=("arial", 11), width=28,
                                       state="readonly")
        self.combo_tipo["values"] = ("Estándard", "Familiar", "Suite")
        self.combo_tipo.current(0)
        self.combo_tipo.grid(row=2, column=1)

        # Botones CRUD
        btn_frame = Frame(frame_form, bg="white")
        btn_frame.place(x=10, y=200, width=470, height=100)

        botones = [("ADD", "black", "yellow"), ("UPDATE", "black", "yellow"),
                   ("DELETE", "black", "yellow"), ("CLEAR", "black", "yellow")]
        for i, (text, bg, fg) in enumerate(botones):
            Button(btn_frame, text=text, font=("arial", 11, "bold"), bg=bg, fg=fg,
                   width=12).grid(row=i, column=0, pady=5)

        # Frame derecho (tabla)
        frame_datos = LabelFrame(self.root, text="Detalles de habitación", font=("arial", 12, "bold"), bg="white", bd=2, relief=RIDGE)
        frame_datos.place(x=520, y=30, width=800, height=440)

        scroll_x = Scrollbar(frame_datos, orient=HORIZONTAL)
        scroll_y = Scrollbar(frame_datos, orient=VERTICAL)

        self.tabla = ttk.Treeview(frame_datos, columns=("Planta", "Nº habitación", "Habitación"),
                                  xscrollcommand=scroll_x.set,
                                  yscrollcommand=scroll_y.set)

        scroll_x.pack(side=BOTTOM, fill=X)
        scroll_y.pack(side=RIGHT, fill=Y)
        scroll_x.config(command=self.tabla.xview)
        scroll_y.config(command=self.tabla.yview)

        self.tabla.heading("Planta", text="Planta")
        self.tabla.heading("Nº habitación", text="Nº habitación")
        self.tabla.heading("Habitación", text="Habitación")
        self.tabla["show"] = "headings"

        for col in ("Planta", "Nº habitación", "Habitación"):
            self.tabla.column(col, width=100)

        self.tabla.pack(fill=BOTH, expand=1)

class Incidencias:
    def __init__(self, parent_frame):
        self.root = parent_frame
        self.root.configure(bg="white")

        self.id_var = StringVar()
        self.habitacion_var = StringVar()
        self.descripcion_var = StringVar()
        self.estado_var = StringVar(value="Pendiente")
        Label(self.root, text="Gestión de Incidencias", font=("times new roman", 18, "bold"),
              bg="black", fg="gold").pack(fill=X)

        Label(self.root, text="Aquí irá el panel de incidencias...", font=("arial", 14)).pack(pady=50)

        Label(self.root, text="Registro de Incidencias", font=("times new roman", 20, "bold"), fg="red", bg="white").place(x=5, y=0)

        frame_form = LabelFrame(self.root, text="Nueva Incidencia", font=("arial", 12, "bold"), bg="white", bd=2, relief=RIDGE)
        frame_form.place(x=5, y=50, width=500, height=250)

        Label(frame_form, text="ID Incidencia", font=("arial", 11, "bold"), bg="white").grid(row=0, column=0, padx=10, pady=10, sticky=W)
        Entry(frame_form, textvariable=self.id_var, font=("arial", 11), width=30).grid(row=0, column=1)

        Label(frame_form, text="Habitación", font=("arial", 11, "bold"), bg="white").grid(row=1, column=0, padx=10, pady=10, sticky=W)
        Entry(frame_form, textvariable=self.habitacion_var, font=("arial", 11), width=30).grid(row=1, column=1)

        Label(frame_form, text="Descripción", font=("arial", 11, "bold"), bg="white").grid(row=2, column=0, padx=10, pady=10, sticky=W)
        Entry(frame_form, textvariable=self.descripcion_var, font=("arial", 11), width=30).grid(row=2, column=1)

        Label(frame_form, text="Estado", font=("arial", 11, "bold"), bg="white").grid(row=3, column=0, padx=10, pady=10, sticky=W)
        combo_estado = ttk.Combobox(frame_form, textvariable=self.estado_var, values=["Pendiente", "Resuelta"], state="readonly", font=("arial", 11), width=28)
        combo_estado.grid(row=3, column=1)

        btn_frame = Frame(frame_form, bg="white")
        btn_frame.place(x=10, y=170, width=470, height=70)

        Button(btn_frame, text="Registrar", command=self.agregar_incidencia, font=("arial", 11, "bold"), bg="black", fg="gold", width=10).grid(row=0, column=0, padx=10)
        Button(btn_frame, text="Eliminar", command=self.eliminar_incidencia, font=("arial", 11, "bold"), bg="black", fg="gold", width=10).grid(row=0, column=1, padx=10)
        Button(btn_frame, text="Reset", command=self.reset_campos, font=("arial", 11, "bold"), bg="black", fg="gold", width=10).grid(row=0, column=2, padx=10)
        Button(btn_frame, text="Marcar como Resuelta", command=self.marcar_resuelta,
               font=("arial", 11, "bold"), bg="black", fg="gold", width=22).grid(row=1, column=0, columnspan=3, pady=5)

        frame_tabla = LabelFrame(self.root, text="Historial de Incidencias", font=("arial", 12, "bold"), bg="white", bd=2, relief=RIDGE)
        frame_tabla.place(x=520, y=50, width=750, height=400)

        scroll_x = Scrollbar(frame_tabla, orient=HORIZONTAL)
        scroll_y = Scrollbar(frame_tabla, orient=VERTICAL)

        self.tabla = ttk.Treeview(frame_tabla, columns=("ID", "Habitación", "Descripción", "Estado"),
                                  xscrollcommand=scroll_x.set,
                                  yscrollcommand=scroll_y.set)

        scroll_x.pack(side=BOTTOM, fill=X)
        scroll_y.pack(side=RIGHT, fill=Y)
        scroll_x.config(command=self.tabla.xview)
        scroll_y.config(command=self.tabla.yview)

        # Filtro por estado
        Label(self.root, text="Filtrar por estado:", font=("arial", 11, "bold"), bg="white").place(x=520, y=460)
        self.filtro_estado_var = StringVar(value="Todos")
        combo_filtro = ttk.Combobox(self.root, textvariable=self.filtro_estado_var,
                                    values=["Todos", "Pendiente", "Resuelta"],
                                    state="readonly", font=("arial", 10), width=18)
        combo_filtro.place(x=660, y=460)
        combo_filtro.bind("<<ComboboxSelected>>", lambda e: self.mostrar_incidencias())

        for col in ("ID", "Habitación", "Descripción", "Estado"):
            self.tabla.heading(col, text=col)
            self.tabla.column(col, width=150)

        self.tabla["show"] = "headings"
        self.tabla.pack(fill=BOTH, expand=1)
        self.tabla.bind("<Double-1>", self.seleccionar_fila)

        self.conectar_bd()
        self.crear_tabla()
        self.mostrar_incidencias()

    def incidencias_view(self):
        for widget in self.right_frame.winfo_children():
            widget.destroy()
        self.app = Incidencias(self.right_frame)

    def conectar_bd(self):
        self.conn = sqlite3.connect("reservas.db")
        self.cursor = self.conn.cursor()

    def crear_tabla(self):
        self.cursor.execute("""
            CREATE TABLE IF NOT EXISTS incidencias (
                id TEXT PRIMARY KEY,
                habitacion TEXT,
                descripcion TEXT,
                estado TEXT
            )
        """)
        self.conn.commit()

    def agregar_incidencia(self):
        id_ = self.id_var.get()
        hab = self.habitacion_var.get()
        desc = self.descripcion_var.get()
        est = self.estado_var.get()

        if not id_ or not hab or not desc:
            messagebox.showwarning("Campos incompletos", "Por favor rellena todos los campos.")
            return

        try:
            self.cursor.execute("INSERT INTO incidencias VALUES (?, ?, ?, ?)", (id_, hab, desc, est))
            self.conn.commit()
            self.mostrar_incidencias()
            messagebox.showinfo("Éxito", "Incidencia registrada.")
        except sqlite3.IntegrityError:
            messagebox.showerror("Error", "Ya existe una incidencia con este ID.")

    def eliminar_incidencia(self):
        id_ = self.id_var.get()
        if not id_:
            messagebox.showwarning("Aviso", "Introduce el ID de la incidencia a eliminar.")
            return
        self.cursor.execute("DELETE FROM incidencias WHERE id=?", (id_,))
        self.conn.commit()
        self.mostrar_incidencias()
        self.reset_campos()

    def mostrar_incidencias(self):
        self.tabla.delete(*self.tabla.get_children())
        filtro = getattr(self, "filtro_estado_var", StringVar(value="Todos")).get()
        if filtro == "Todos":
            self.cursor.execute("SELECT * FROM incidencias")
        else:
            self.cursor.execute("SELECT * FROM incidencias WHERE estado=?", (filtro,))
        for fila in self.cursor.fetchall():
            self.tabla.insert("", END, values=fila)

    def seleccionar_fila(self, event):
        item = self.tabla.focus()
        valores = self.tabla.item(item, "values")
        if valores:
            self.id_var.set(valores[0])
            self.habitacion_var.set(valores[1])
            self.descripcion_var.set(valores[2])
            self.estado_var.set(valores[3])

    def reset_campos(self):
        self.id_var.set("")
        self.habitacion_var.set("")
        self.descripcion_var.set("")
        self.estado_var.set("Pendiente")

    def marcar_resuelta(self):
        id_ = self.id_var.get()
        if not id_:
            messagebox.showwarning("Aviso", "Selecciona o introduce el ID de una incidencia.")
            return

        self.cursor.execute("SELECT * FROM incidencias WHERE id=?", (id_,))
        if not self.cursor.fetchone():
            messagebox.showerror("Error", "No se encontró ninguna incidencia con ese ID.")
            return

        self.cursor.execute("UPDATE incidencias SET estado='Resuelta' WHERE id=?", (id_,))
        self.conn.commit()
        self.mostrar_incidencias()
        self.estado_var.set("Resuelta")
        messagebox.showinfo("Actualizado", "✅ Incidencia marcada como resuelta.")


# Ejecución principal
if __name__ == "__main__":
    root = Tk()
    obj = HotelManagementSystem(root)
    root.mainloop()
