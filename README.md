from producto import Producto
from inventario import Inventario

def main():
    """
    Función principal que ejecuta el menú interactivo en la consola.
    """
    inventario = Inventario()

    # Agregamos algunos productos de víveres como ejemplo
    productos_iniciales = [
        Producto("1", "Arroz", 50, 1.50),
        Producto("2", "Frijol", 30, 2.00),
        Producto("3", "Azúcar", 20, 0.80),
        Producto("4", "Aceite", 15, 3.50),
        Producto("5", "Harina", 25, 1.20)
    ]
    for producto in productos_iniciales:
        inventario.agregar_producto(producto)

    while True:
        print("\n----- Menú de Inventario -----")
        print("1. Agregar Producto")
        print("2. Eliminar Producto")
        print("3. Actualizar Producto")
        print("4. Buscar Producto")
        print("5. Mostrar todos los productos")
        print("6. Salir")
        opcion = input("Seleccione una opción: ")

        if opcion == "1":
            # Agregar producto
            id = input("Ingrese el ID del producto: ")
            nombre = input("Ingrese el nombre del producto: ")
            try:
                cantidad = int(input("Ingrese la cantidad: "))
                precio = float(input("Ingrese el precio: "))
            except ValueError:
                print("Error: La cantidad debe ser un entero y el precio un número.")
                continue

            producto = Producto(id, nombre, cantidad, precio)
            inventario.agregar_producto(producto)

        elif opcion == "2":
            # Eliminar producto
            id = input("Ingrese el ID del producto a eliminar: ")
            inventario.eliminar_producto(id)

        elif opcion == "3":
            # Actualizar producto
            id = input("Ingrese el ID del producto a actualizar: ")
            # Verificamos si el producto existe
            producto_existente = None
            for p in inventario.productos:
                if p.get_id() == id:
                    producto_existente = p
                    break

            if producto_existente:
                print("Deje en blanco si no desea actualizar un campo.")
                cantidad_input = input(f"Ingrese la nueva cantidad (actual {producto_existente.get_cantidad()}): ")
                precio_input = input(f"Ingrese el nuevo precio (actual {producto_existente.get_precio()}): ")

                # Si el usuario deja el campo en blanco, se mantiene el valor actual
                try:
                    cantidad = int(cantidad_input) if cantidad_input.strip() != "" else producto_existente.get_cantidad()
                    precio = float(precio_input) if precio_input.strip() != "" else producto_existente.get_precio()
                except ValueError:
                    print("Error: Ingrese valores numéricos válidos para cantidad y precio.")
                    continue

                inventario.actualizar_producto(id, cantidad, precio)
            else:
                print(f"Producto con ID {id} no encontrado.")

        elif opcion == "4":
            # Buscar producto
            nombre = input("Ingrese el nombre del producto a buscar: ")
            resultados = inventario.buscar_producto(nombre)
            if resultados:
                print("Productos encontrados:")
                for p in resultados:
                    print(p)
            else:
                print("No se encontraron productos con ese nombre.")

        elif opcion == "5":
            # Mostrar todos los productos
            inventario.mostrar_productos()

        elif opcion == "6":
            # Salir del programa
            print("Saliendo del programa...")
            break

        else:
            print("Opción no válida, por favor intente de nuevo.")

if __name__ == "__main__":
    main()
