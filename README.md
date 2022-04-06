# Automatización Liquidos 1A S.A.
## Diseño

### Tabla de variables
| Variable    | Atributo | Tipo | Descripción                              |
| ----------- | -------- | ---- | ---------------------------------------- |
| Start       | Input    | Bool | Inicio                                   |
| Motor1      | Output   | Bool | Motor 1                                  |
| Stop        | Input    | Bool | Parado de emergencia                     |
| Motor2      | Output   | Bool | Motor 2                                  |
| Prop1       | Output   | Bool | Helice de mezcla                         |
| Reset       | Input    | Bool | Parar y reiniciar el conteo              |
| ContMil1    | Input    | Bool | Sensor lleno Contenedor 1 de 1000 Litros |
| ContMil2    | Input    | Bool | Sensor lleno Contenedor 2 de 1000 Litros |
| ContFin1    | Input    | Bool | Sensor lleno Contenedor de 2000 Litros   |
| Valve1      | Output   | Bool | Valvula 1                                |
| Valve2      | Output   | Bool | Valvula 2                                |
| Valve3      | Output   | Bool | Valvula 3                                |
| EmptyConFin | Input    | Bool | Sensor vacío contenedor 2000 Litros      |

![Diagrama Función Secuencial](diagramaSeq.png)

## Desarrollo
## Implementacion