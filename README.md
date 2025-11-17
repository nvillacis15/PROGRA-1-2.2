# PROGRA-1-2.2
#include <stdio.h>
#include <string.h>

#define MAX_PRODUCTOS 50
#define MAX_NOMBRE 50

// Variables globales para almacenar datos
char nombres[MAX_PRODUCTOS][MAX_NOMBRE];
int cantidades[MAX_PRODUCTOS];
float tiempoUnitario[MAX_PRODUCTOS];  // Tiempo para fabricar 1 unidad
float recursosUnitarios[MAX_PRODUCTOS]; // Recursos para fabricar 1 unidad
int totalProductos = 0;

// Prototipos de funciones
void menuPrincipal();
void ingresarProducto();
void editarProducto();
void eliminarProducto();
void calcularProduccion();
void mostrarProductos();
int buscarProducto(char *nombre);

int main() {
    int opcion;
    
    printf("=== SISTEMA DE GESTION DE PRODUCCION ===\n\n");
    
    do {
        menuPrincipal();
        printf("Seleccione una opcion: ");
        scanf("%d", &opcion);
        while(getchar() != '\n'); // Limpiar buffer después de scanf
        
        switch(opcion) {
            case 1:
                ingresarProducto();
                break;
            case 2:
                editarProducto();
                break;
            case 3:
                eliminarProducto();
                break;
            case 4:
                calcularProduccion();
                break;
            case 5:
                mostrarProductos();
                break;
            case 6:
                printf("\nSaliendo del sistema...\n");
                break;
            default:
                printf("\nOpcion invalida. Intente nuevamente.\n");
        }
        
        if(opcion != 6) {
            printf("\nPresione Enter para continuar...");
            getchar();
        }
        
    } while(opcion != 6);
    
    return 0;
}

void menuPrincipal() {
    printf("\n================================\n");
    printf("         MENU PRINCIPAL\n");
    printf("================================\n");
    printf("1. Ingresar producto\n");
    printf("2. Editar producto\n");
    printf("3. Eliminar producto\n");
    printf("4. Calcular produccion\n");
    printf("5. Mostrar todos los productos\n");
    printf("6. Salir\n");
    printf("================================\n");
}

void ingresarProducto() {
    int i, tieneNumero;
    
    if(totalProductos >= MAX_PRODUCTOS) {
        printf("\nNo se pueden agregar mas productos. Limite alcanzado.\n");
        return;
    }
    
    printf("\n--- INGRESAR NUEVO PRODUCTO ---\n");
    
    // Validar que el nombre no contenga números
    do {
        tieneNumero = 0;
        printf("Nombre del producto (solo letras y espacios): ");
        fgets(nombres[totalProductos], MAX_NOMBRE, stdin);
        nombres[totalProductos][strcspn(nombres[totalProductos], "\n")] = 0;
        
        // Verificar si hay números
        for(i = 0; nombres[totalProductos][i] != '\0'; i++) {
            if(nombres[totalProductos][i] >= '0' && nombres[totalProductos][i] <= '9') {
                tieneNumero = 1;
                break;
            }
        }
        
        if(tieneNumero) {
            printf("ERROR: El nombre no puede contener numeros. Intente nuevamente.\n\n");
        }
        
    } while(tieneNumero);
    
    // Validar cantidad (no negativa)
    do {
        printf("Cantidad demandada: ");
        scanf("%d", &cantidades[totalProductos]);
        
        if(cantidades[totalProductos] < 0) {
            printf("ERROR: La cantidad no puede ser negativa. Intente nuevamente.\n\n");
        }
    } while(cantidades[totalProductos] < 0);
    
    // Validar tiempo unitario (no negativo)
    do {
        printf("Tiempo de fabricacion por unidad (horas): ");
        scanf("%f", &tiempoUnitario[totalProductos]);
        
        if(tiempoUnitario[totalProductos] < 0) {
            printf("ERROR: El tiempo no puede ser negativo. Intente nuevamente.\n\n");
        }
    } while(tiempoUnitario[totalProductos] < 0);
    
    // Validar recursos unitarios (no negativos)
    do {
        printf("Recursos necesarios por unidad: ");
        scanf("%f", &recursosUnitarios[totalProductos]);
        
        if(recursosUnitarios[totalProductos] < 0) {
            printf("ERROR: Los recursos no pueden ser negativos. Intente nuevamente.\n\n");
        }
    } while(recursosUnitarios[totalProductos] < 0);
    
    while(getchar() != '\n'); // Limpiar buffer
    
    totalProductos++;
    printf("\nProducto agregado exitosamente!\n");
}

void editarProducto() {
    char nombreBuscar[MAX_NOMBRE];
    int indice, opcion;
    
    if(totalProductos == 0) {
        printf("\nNo hay productos registrados.\n");
        return;
    }
    
    printf("\n--- EDITAR PRODUCTO ---\n");
    printf("Ingrese el nombre del producto a editar: ");
    fgets(nombreBuscar, MAX_NOMBRE, stdin);
    nombreBuscar[strcspn(nombreBuscar, "\n")] = 0;
    
    indice = buscarProducto(nombreBuscar);
    
    if(indice == -1) {
        printf("\nProducto no encontrado.\n");
        return;
    }
    
    printf("\nProducto encontrado:\n");
    printf("Nombre: %s\n", nombres[indice]);
    printf("Cantidad: %d\n", cantidades[indice]);
    printf("Tiempo unitario: %.2f horas\n", tiempoUnitario[indice]);
    printf("Recursos unitarios: %.2f\n", recursosUnitarios[indice]);
    
    printf("\n¿Que desea editar?\n");
    printf("1. Nombre\n");
    printf("2. Cantidad\n");
    printf("3. Tiempo unitario\n");
    printf("4. Recursos unitarios\n");
    printf("5. Cancelar\n");
    printf("Opcion: ");
    scanf("%d", &opcion);
    while(getchar() != '\n'); // Limpiar buffer
    
    switch(opcion) {
        case 1:
            printf("Nuevo nombre: ");
            fgets(nombres[indice], MAX_NOMBRE, stdin);
            nombres[indice][strcspn(nombres[indice], "\n")] = 0;
            break;
        case 2:
            do {
                printf("Nueva cantidad: ");
                scanf("%d", &cantidades[indice]);
                if(cantidades[indice] < 0) {
                    printf("ERROR: La cantidad no puede ser negativa.\n\n");
                }
            } while(cantidades[indice] < 0);
            while(getchar() != '\n');
            break;
        case 3:
            do {
                printf("Nuevo tiempo unitario: ");
                scanf("%f", &tiempoUnitario[indice]);
                if(tiempoUnitario[indice] < 0) {
                    printf("ERROR: El tiempo no puede ser negativo.\n\n");
                }
            } while(tiempoUnitario[indice] < 0);
            while(getchar() != '\n');
            break;
        case 4:
            do {
                printf("Nuevos recursos unitarios: ");
                scanf("%f", &recursosUnitarios[indice]);
                if(recursosUnitarios[indice] < 0) {
                    printf("ERROR: Los recursos no pueden ser negativos.\n\n");
                }
            } while(recursosUnitarios[indice] < 0);
            while(getchar() != '\n');
            break;
        case 5:
            printf("Operacion cancelada.\n");
            return;
        default:
            printf("Opcion invalida.\n");
            return;
    }
    
    printf("\nProducto editado exitosamente!\n");
}

void eliminarProducto() {
    char nombreBuscar[MAX_NOMBRE];
    int indice, i;
    
    if(totalProductos == 0) {
        printf("\nNo hay productos registrados.\n");
        return;
    }
    
    printf("\n--- ELIMINAR PRODUCTO ---\n");
    printf("Ingrese el nombre del producto a eliminar: ");
    fgets(nombreBuscar, MAX_NOMBRE, stdin);
    nombreBuscar[strcspn(nombreBuscar, "\n")] = 0;
    
    indice = buscarProducto(nombreBuscar);
    
    if(indice == -1) {
        printf("\nProducto no encontrado.\n");
        return;
    }
    
    printf("\nProducto encontrado: %s\n", nombres[indice]);
    printf("¿Esta seguro de eliminar este producto? (1=Si, 0=No): ");
    
    int confirmacion;
    scanf("%d", &confirmacion);
    while(getchar() != '\n'); // Limpiar buffer
    
    if(confirmacion != 1) {
        printf("Operacion cancelada.\n");
        return;
    }
    
    // Desplazar elementos hacia la izquierda
    for(i = indice; i < totalProductos - 1; i++) {
        strcpy(nombres[i], nombres[i + 1]);
        cantidades[i] = cantidades[i + 1];
        tiempoUnitario[i] = tiempoUnitario[i + 1];
        recursosUnitarios[i] = recursosUnitarios[i + 1];
    }
    
    totalProductos--;
    printf("\nProducto eliminado exitosamente!\n");
}

void calcularProduccion() {
    float tiempoTotal = 0;
    float recursosTotal = 0;
    float tiempoDisponible, recursosDisponibles;
    int i;
    
    if(totalProductos == 0) {
        printf("\nNo hay productos registrados.\n");
        return;
    }
    
    printf("\n--- CALCULO DE PRODUCCION ---\n\n");
    
    // Calcular totales
    for(i = 0; i < totalProductos; i++) {
        tiempoTotal += cantidades[i] * tiempoUnitario[i];
        recursosTotal += cantidades[i] * recursosUnitarios[i];
    }
    
    printf("RESULTADOS:\n");
    printf("===========\n");
    printf("1. Tiempo total de fabricacion: %.2f horas\n", tiempoTotal);
    printf("2. Recursos totales necesarios: %.2f unidades\n\n", recursosTotal);
    
    // Detalle por producto
    printf("DETALLE POR PRODUCTO:\n");
    printf("=====================\n");
    for(i = 0; i < totalProductos; i++) {
        printf("Producto: %s\n", nombres[i]);
        printf("  - Cantidad: %d unidades\n", cantidades[i]);
        printf("  - Tiempo requerido: %.2f horas\n", cantidades[i] * tiempoUnitario[i]);
        printf("  - Recursos requeridos: %.2f unidades\n\n", cantidades[i] * recursosUnitarios[i]);
    }
    
    // Verificar disponibilidad
    printf("VERIFICACION DE DISPONIBILIDAD:\n");
    printf("================================\n");
    
    do {
        printf("Ingrese el tiempo disponible (horas): ");
        scanf("%f", &tiempoDisponible);
        if(tiempoDisponible < 0) {
            printf("ERROR: El tiempo no puede ser negativo.\n\n");
        }
    } while(tiempoDisponible < 0);
    
    do {
        printf("Ingrese los recursos disponibles: ");
        scanf("%f", &recursosDisponibles);
        if(recursosDisponibles < 0) {
            printf("ERROR: Los recursos no pueden ser negativos.\n\n");
        }
    } while(recursosDisponibles < 0);
    
    while(getchar() != '\n'); // Limpiar buffer
    
    printf("\nANALISIS:\n");
    if(tiempoTotal <= tiempoDisponible && recursosTotal <= recursosDisponibles) {
        printf("✓ LA FABRICA PUEDE CUMPLIR CON LA DEMANDA\n");
        printf("  - Tiempo sobrante: %.2f horas\n", tiempoDisponible - tiempoTotal);
        printf("  - Recursos sobrantes: %.2f unidades\n", recursosDisponibles - recursosTotal);
    } else {
        printf("✗ LA FABRICA NO PUEDE CUMPLIR CON LA DEMANDA\n");
        if(tiempoTotal > tiempoDisponible) {
            printf("  - Faltan %.2f horas de produccion\n", tiempoTotal - tiempoDisponible);
        }
        if(recursosTotal > recursosDisponibles) {
            printf("  - Faltan %.2f unidades de recursos\n", recursosTotal - recursosDisponibles);
        }
    }
}

void mostrarProductos() {
    int i;
    
    if(totalProductos == 0) {
        printf("\nNo hay productos registrados.\n");
        return;
    }
    
    printf("\n--- LISTA DE PRODUCTOS ---\n\n");
    for(i = 0; i < totalProductos; i++) {
        printf("Producto %d:\n", i + 1);
        printf("  Nombre: %s\n", nombres[i]);
        printf("  Cantidad demandada: %d\n", cantidades[i]);
        printf("  Tiempo unitario: %.2f horas\n", tiempoUnitario[i]);
        printf("  Recursos unitarios: %.2f\n", recursosUnitarios[i]);
        printf("  ---------------------------\n");
    }
    printf("Total de productos: %d\n", totalProductos);
}

int buscarProducto(char *nombre) {
    int i, j;
    int encontrado;
    
    for(i = 0; i < totalProductos; i++) {
        encontrado = 1;
        
        // Comparar caracter por caracter (lógica propia sin strcmp)
        for(j = 0; nombre[j] != '\0' || nombres[i][j] != '\0'; j++) {
            if(nombre[j] != nombres[i][j]) {
                encontrado = 0;
                break;
            }
        }
        
        if(encontrado) {
            return i;
        }
    }
    
    return -1; // No encontrado
}
