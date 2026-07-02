# Trabajo-IA
import json
import os

ARCHIVO = "tareas.json"


def cargar_tareas():
    if os.path.exists(ARCHIVO):
        with open(ARCHIVO, "r", encoding="utf-8") as f:
            return json.load(f)
    return []


def guardar_tareas(tareas):
    with open(ARCHIVO, "w", encoding="utf-8") as f:
        json.dump(tareas, f, indent=4, ensure_ascii=False)


def agregar_tarea(tareas):
    descripcion = input("Ingrese la descripción de la tarea: ")

    tarea = {
        "descripcion": descripcion,
        "completada": False
    }

    tareas.append(tarea)
    guardar_tareas(tareas)
    print("Tarea agregada correctamente.\n")


def mostrar_tareas(tareas):
    if not tareas:
        print("\nNo existen tareas.\n")
        return

    print("\n===== LISTA DE TAREAS =====")

    for i, tarea in enumerate(tareas, start=1):
        estado = "✔" if tarea["completada"] else "✘"
        print(f"{i}. [{estado}] {tarea['descripcion']}")

    print()


def completar_tarea(tareas):
    mostrar_tareas(tareas)

    if not tareas:
        return

    try:
        indice = int(input("Número de tarea completada: ")) - 1

        if 0 <= indice < len(tareas):
            tareas[indice]["completada"] = True
            guardar_tareas(tareas)
            print("Tarea actualizada.\n")
        else:
            print("Número inválido.\n")

    except ValueError:
        print("Debe ingresar un número.\n")


def eliminar_tarea(tareas):
    mostrar_tareas(tareas)

    if not tareas:
        return

    try:
        indice = int(input("Número de tarea a eliminar: ")) - 1

        if 0 <= indice < len(tareas):
            tareas.pop(indice)
            guardar_tareas(tareas)
            print("Tarea eliminada.\n")
        else:
            print("Número inválido.\n")

    except ValueError:
        print("Debe ingresar un número.\n")


def menu():
    tareas = cargar_tareas()

    while True:

        print("===== LISTA DE TAREAS =====")
        print("1. Agregar tarea")
        print("2. Mostrar tareas")
        print("3. Completar tarea")
        print("4. Eliminar tarea")
        print("5. Salir")

        opcion = input("Seleccione una opción: ")

        if opcion == "1":
            agregar_tarea(tareas)

        elif opcion == "2":
            mostrar_tareas(tareas)

        elif opcion == "3":
            completar_tarea(tareas)

        elif opcion == "4":
            eliminar_tarea(tareas)

        elif opcion == "5":
            print("Hasta luego.")
            break

        else:
            print("Opción inválida.\n")


if __name__ == "__main__":
    menu()