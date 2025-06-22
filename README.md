# Gestion_Hotelera
import random
import string
from faker import Faker
import sqlite3

class DatabaseManager:
    def _init_(self, db_name="hotel.db"):
        self.db_name = db_name
        self.create_table()
#sdfsdf
    def create_table(self):
        conn = sqlite3.connect(self.db_name)
        cursor = conn.cursor()
        cursor.execute("""
            CREATE TABLE IF NOT EXISTS clientes (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                ref TEXT,
                nombre TEXT,
                madre TEXT,
                genero TEXT,
                postal TEXT,
                movil TEXT,
                email TEXT,
                nacionalidad TEXT,
                doc_tipo TEXT,
                doc_num TEXT,
                direccion TEXT
            )
        """)
        conn.commit()
        conn.close()

    def insertar_cliente(self, datos):
        try:
            conn = sqlite3.connect(self.db_name)
            cursor = conn.cursor()
            cursor.execute("""
                INSERT INTO clientes (
                    ref, nombre, madre, genero, postal, movil, email,
                    nacionalidad, doc_tipo, doc_num, direccion
                ) VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
            """, tuple(datos.values()))
            conn.commit()
            conn.close()
            return True, "Cliente guardado con éxito."
        except Exception as e:
            return False, str(e)

import os

def generar_ref():
    return ''.join(random.choices(string.ascii_uppercase + string.digits, k=8))

def simular_clientes(n=10):
    fake = Faker('es_ES')
    db = DatabaseManager("hotel.db")

    for _ in range(n):
        datos = {
            "ref": generar_ref(),
            "nombre": fake.first_name() + " " + fake.last_name(),
            "madre": fake.first_name() + " " + fake.last_name(),
            "genero": random.choice(["Male", "Female", "Other"]),
            "postal": fake.postcode(),
            "movil": fake.phone_number(),
            "email": fake.email(),
            "nacionalidad": fake.country(),
            "doc_tipo": random.choice(["DNI", "Pasaporte", "NIE"]),
            "doc_num": fake.bothify(text='??######'),
            "direccion": fake.address().replace("\n", ", ")
        }
        exito, mensaje = db.insertar_cliente(datos)
        print(mensaje if exito else f"Error: {mensaje}")

if _name_ == "_main_":
    simular_clientes(20)
