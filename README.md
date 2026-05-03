import random

historial_total = []

def lanzar_dados(cantidad):
    # Distribución uniforme discreta ( Probabilidad)
    dados = [random.randint(1, 6) for _ in range(cantidad)]
    historial_total.extend(dados)
    return dados

def calcular_puntos(dados):

    return sum(dados)

class Jugador:
    def __init__(self, nombre):
        self.nombre = nombre

    def jugar_turno(self):
        print(f"\n--- Turno de {self.nombre} ---")
        dados = lanzar_dados(5)
        lanzamientos = 1

        while lanzamientos < 3:
            print(f"Lanzamiento {lanzamientos}: {dados}")
            quedarse = input("Índices a mantener (ej: 0 2 4) o 'todos': ")
            if quedarse.lower() == 'todos': break


            try:
                indices = [int(i) for i in quedarse.split()]
                nuevos_dados = lanzar_dados(5 - len(indices))
                dados_mantenidos = [dados[i] for i in indices]
                dados = dados_mantenidos + nuevos_dados
                lanzamientos += 1
            except:
                print("Error: Ingresa los números separados por espacios (ej: 0 1 4)")

        print(f"Resultado final del turno: {dados}")
        return calcular_puntos(dados)

def mostrar_estadisticas():
    print("\n" + "="*35)
    print("  ANÁLISIS ESTADÍSTICO FINAL  ")
    print("="*35)
    total = len(historial_total)
    print(f"Total de dados lanzados: {total}")
    print("-" * 35)
    print(f"{'Cara':<10} | {'Frecuencia':<12} | {'Probabilidad %'}")
    print("-" * 35)
    for cara in range(1, 7):
        frec = historial_total.count(cara)
        prob = (frec / total) * 100 if total > 0 else 0
        print(f"Cara {cara:<5} | {frec:<12} | {prob:.2f}%")
    print("-" * 35)
    print("Análisis: Los valores tienden al 16.67% (Distribución Uniforme).")

def simular_yahtzee():
    j1 = Jugador("Jugador 1")
    j2 = Jugador("Jugador 2")

    for ronda in range(1, 3): # 2 rondas de simulación
        print(f"\n=== RONDA {ronda} ===")
        p1 = j1.jugar_turno()
        p2 = j2.jugar_turno()
        print(f"Marcador Ronda {ronda}: J1: {p1} | J2: {p2}")

if __name__ == "__main__":
    simular_yahtzee()
    mostrar_estadisticas() # <--- Esto generará la tabla al final
