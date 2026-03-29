pythonCopiarEditarimport random

# Definir parámetros del algoritmo
tamaño_poblacion = 100
num_generaciones = 50
tasa_mutacion = 0.01
tasa_cruzamiento = 0.7

# Representar materiales educativos y estudiantes
materiales_educativos = ['Curso A', 'Curso B', 'Curso C', 'Curso D', 'Curso E']
estudiantes = []

# Crear la población inicial de estudiantes con trayectorias educativas aleatorias
def crear_individuo():
    return random.sample(materiales_educativos, len(materiales_educativos))

def inicializar_poblacion():
    return [crear_individuo() for _ in range(tamaño_poblacion)]

# Función de aptitud (fitness) que evalúa el rendimiento del estudiante
def evaluar_individuo(individuo):
    # Simulamos el rendimiento del estudiante (esto puede ser sustituido por un sistema de IA real)
    rendimiento = 0
    for material in individuo:
        if material == 'Curso A':  # Ejemplo de material adecuado para el estudiante
            rendimiento += 10
        elif material == 'Curso B':  # Ejemplo de material menos adecuado
            rendimiento += 5
        else:
            rendimiento += 1
    return rendimiento

# Selección de los mejores individuos (tournament selection)
def seleccion(poblacion):
    poblacion_ordenada = sorted(poblacion, key=lambda x: evaluar_individuo(x), reverse=True)
    return poblacion_ordenada[:int(tamaño_poblacion * tasa_cruzamiento)]

# Cruzamiento (Crossover) entre dos individuos
def cruzar(padre1, padre2):
    punto_cruce = random.randint(1, len(padre1) - 1)
    hijo1 = padre1[:punto_cruce] + padre2[punto_cruce:]
    hijo2 = padre2[:punto_cruce] + padre1[punto_cruce:]
    return hijo1, hijo2

# Mutación: modificar aleatoriamente un individuo
def mutar(individuo):
    if random.random() < tasa_mutacion:
        idx = random.randint(0, len(individuo) - 1)
        nuevo_material = random.choice(materiales_educativos)
        individuo[idx] = nuevo_material
    return individuo

# Algoritmo Genético
def algoritmo_genetico():
    poblacion = inicializar_poblacion()
    
    for generacion in range(num_generaciones):
        print(f"Generación {generacion}")
        
        # Seleccionar los mejores individuos
        mejores_individuos = seleccion(poblacion)
        
        # Crear una nueva población
        nueva_poblacion = []
        
        # Cruzamiento y mutación
        while len(nueva_poblacion) < tamaño_poblacion:
            padre1, padre2 = random.sample(mejores_individuos, 2)
            hijo1, hijo2 = cruzar(padre1, padre2)
            nueva_poblacion.append(mutar(hijo1))
            nueva_poblacion.append(mutar(hijo2))
        
        # Reemplazar la población actual con la nueva
        poblacion = nueva_poblacion
    
    # Devolver el mejor individuo de la última generación
    mejor_individuo = max(poblacion, key=lambda x: evaluar_individuo(x))
    return mejor_individuo

# Ejecutar el algoritmo genético
mejor_ruta_de_aprendizaje = algoritmo_genetico()
print("La mejor ruta de aprendizaje es:", mejor_ruta_de_aprendizaje)
