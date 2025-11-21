#include <stdio.h>
#include <string.h>

#define MAX 50

// Arreglos globales SIN PUNTEROS
char nombres[MAX][30];
int cantidades[MAX];
float precios[MAX];
int totalProductos = 0;

int leerEntero(const char *mensaje) {
    int numeroIngresado;
    int resultadoLectura;
    char caracter;

    do {
        printf("%s", mensaje);
        resultadoLectura = scanf("%d", &numeroIngresado);

        if (resultadoLectura == 1) {
            while ((caracter = getchar()) != '\n');
            return numeroIngresado;
        }

        printf("ERROR: Ingrese solo números enteros.\n");
        while ((caracter = getchar()) != '\n');
    } while (1);
}

float leerFloat(const char *mensaje) {
    float numeroIngresado;
    int resultadoLectura;
    char caracter;

    do {
        printf("%s", mensaje);
        resultadoLectura = scanf("%f", &numeroIngresado);

        if (resultadoLectura == 1) {
            while ((caracter = getchar()) != '\n');
            return numeroIngresado;
        }

        printf("ERROR: Ingrese un número válido.\n");
        while ((caracter = getchar()) != '\n');
    } while (1);
}

void leerTexto(const char *mensaje, char destino[]) {
    char caracter;

    printf("%s", mensaje);
    scanf("%29s", destino);

    while ((caracter = getchar()) != '\n');
}


void agregarProducto() {
    if (totalProductos >= MAX) {
        printf("Inventario lleno.\n");
        return;
    }

    printf("\n--- Agregar producto ---\n");
    leerTexto("Nombre: ", nombres[totalProductos]);
    cantidades[totalProductos] = leerEntero("Cantidad: ");
    precios[totalProductos] = leerFloat("Precio: ");

    totalProductos++;
    printf("Producto agregado exitosamente.\n");
}

void mostrarInventario() {
    if (totalProductos == 0) {
        printf("No hay productos registrados.\n");
        return;
    }

    printf("\n--- Inventario ---\n");
    for (int i = 0; i < totalProductos; i++) {
        printf("%d) %s - Cantidad: %d - Precio: %.2f\n",
               i + 1, nombres[i], cantidades[i], precios[i]);
    }
}

void editarProducto() {
    if (totalProductos == 0) {
        printf("No hay productos para editar.\n");
        return;
    }

    mostrarInventario();
    int indice = leerEntero("Elija el número del producto a editar: ") - 1;

    if (indice < 0 || indice >= totalProductos) {
        printf("Índice inválido.\n");
        return;
    }

    printf("Editando: %s\n", nombres[indice]);
    leerTexto("Nuevo nombre: ", nombres[indice]);
    cantidades[indice] = leerEntero("Nueva cantidad: ");
    precios[indice] = leerFloat("Nuevo precio: ");

    printf("Producto editado correctamente.\n");
}

void eliminarProducto() {
    if (totalProductos == 0) {
        printf("Inventario vacío.\n");
        return;
    }

    mostrarInventario();
    int indice = leerEntero("Seleccione el número del producto a eliminar: ") - 1;

    if (indice < 0 || indice >= totalProductos) {
        printf("Índice inválido.\n");
        return;
    }


    for (int i = indice; i < totalProductos - 1; i++) {
        strcpy(nombres[i], nombres[i + 1]);
        cantidades[i] = cantidades[i + 1];
        precios[i] = precios[i + 1];
    }

    totalProductos--;
    printf("Producto eliminado.\n");
}

void calcularProduccion() {
    if (totalProductos == 0) {
        printf("No hay productos para calcular.\n");
        return;
    }

    float costoTotal = 0;
    for (int i = 0; i < totalProductos; i++) {
        costoTotal += cantidades[i] * precios[i];
    }

    printf("\nCosto total de producción: %.2f\n", costoTotal);
}


int main() {
    int opcion;

    do {
        printf("\n===== MENU PRINCIPAL =====\n");
        printf("1. Agregar producto\n");
        printf("2. Mostrar inventario\n");
        printf("3. Editar producto\n");
        printf("4. Eliminar producto\n");
        printf("5. Calcular costo de producción\n");
        printf("6. Salir\n");

        opcion = leerEntero("Seleccione una opción: ");

        switch (opcion) {
            case 1: agregarProducto(); break;
            case 2: mostrarInventario(); break;
            case 3: editarProducto(); break;
            case 4: eliminarProducto(); break;
            case 5: calcularProduccion(); break;
            case 6: printf("Saliendo...\n"); break;
            default: printf("Opción inválida. Intente nuevamente.\n");
        }

    } while (opcion != 6);

    return 0;
}

