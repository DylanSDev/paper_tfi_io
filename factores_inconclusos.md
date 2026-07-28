# Factores que actualmente resultan inconclusos

A continuación se detallan los 14 factores del modelo de pronósticos y optimización por metas lexicográfica que requieren definición, formalización o justificación metodológica. Cada factor incluye sus aspectos pendientes, la consecuencia de su omisión y su correspondiente lista de verificación comprobable.

---

## 1. Conversión del Pronóstico

- **Aspecto no definido:** La planilla de pronósticos proyecta aproximadamente 2.522 casos de dengue, 678 de bronquiolitis y 632 de influenza para la Semana Epidemiológica 19. Sin embargo, el modelo de optimización utiliza 346, 95 y 89 pacientes mediante fórmulas del tipo $\text{pronóstico} \times 0,7 / 5$. No se explica técnicamente en el informe qué representa el 70 %, por qué se divide entre cinco hospitales ni por qué dengue utiliza 2.471 en lugar de 2.522.
- **Consecuencia:** La demanda hospitalaria puede parecer arbitraria y existe riesgo de trabajar con versiones diferentes o no unificadas del pronóstico.

### Lista de verificación
- [x] Documentar explícitamente el origen y justificación del factor 0,60 (porcentaje de demanda dependiente del subsistema público de salud SIPROSA durante picos de contingencia).
- [x] Justificar la división entre 6 efectores principales de tercer nivel de la red provincial de Tucumán.
- [x] Unificar y conciliar el valor base de proyección de demanda entre las planillas de soporte y la entrada del modelo GP.
- [x] Formalizar la ecuación matemática de conversión $D_i = \left\lceil \frac{F_{i, t+m} \cdot \theta_{\text{público}}}{H_{\text{nodales}}} \right\rceil$ en el texto del informe.

---

## 2. Alcance Institucional

- **Aspecto no definido:** El informe habla genéricamente de los "hospitales públicos de Tucumán", mientras que la planilla de parámetros indica que las capacidades se calculan específicamente para el Hospital Centro de Salud Zenón J. Santillán.
- **Consecuencia:** No queda claro si el modelo representa un único hospital nodal, una red de hospitales o una proporción de la capacidad provincial.

### Lista de verificación
- [x] Aclarar en el informe que el modelo es un marco general que se alimenta primero por el modelo de pronósticos y luego por los parámetros de un hospital en específico (herramienta matemática de soporte a decisiones, no una regla fija).
- [x] Aclarar que para probar y validar el funcionamiento del modelo se utilizaron datos del Hospital Centro de Salud Zenón J. Santillán.
- [x] Detallar que los datos utilizados combinan valores confirmados y estimados (cuyas estimaciones y supuestos se detallan en la planilla de Excel).

---

## 3. Hospitalización y Severidad

- **Aspecto no definido:** El informe presenta tasas específicas de hospitalización (5%–7% Dengue, 10%–15% Bronquiolitis, 8%–12% Influenza) y cuidados críticos por patología, pero estas tasas no aparecen explícitamente en la fórmula de conversión de casos a la demanda $D_i$ del modelo.
- **Consecuencia:** No puede demostrarse matemáticamente que los 346, 95 y 89 pacientes sean realmente quienes requieren la infraestructura hospitalaria.

### Lista de verificación
- [ ] Integrar formalmente las tasas de triaje y hospitalización en la ecuación de conversión de la demanda.
- [ ] Demostrar que los valores $D_i = (346, 95, 89)$ se derivan de aplicar los porcentajes de severidad sobre el total de contagios proyectados.
- [ ] Incluir la formulación explícita del filtro epidemiológico: $D_{i,\text{hosp}} = \text{Casos}_i \times \text{TasaHosp}_i$.

---

## 4. Paciente Homogéneo

- **Aspecto no definido:** Todos los pacientes atendidos de una misma patología consumen el mismo paquete promedio de recursos. Por ejemplo, cada caso de influenza atendido genera consumo simultáneo de camas, oxígeno, kits y personal.
- **Consecuencia:** Se mezclan pacientes ambulatorios, internados en sala general y pacientes críticos de UTI en una única variable $X_i$.

### Lista de verificación
- [ ] Explicar y justificar la asunción de "paciente promedio ponderado" basada en la distribución histórica de severidad.
- [ ] Documentar el cálculo de los coeficientes tecnológicos promedio $T_{i,r}$ como ponderación entre casos moderados y graves.
- [ ] Proponer en las recomendaciones la futura desagregación de $X_i$ en subcategorías por nivel de atención (ambulatorio, sala general, UTI).

---

## 5. Ponderadores Clínicos ($\mu_i$)

- **Aspecto no definido:** Los valores de gravedad en la Meta 1 son $\mu_1 = 1$ para dengue, $\mu_2 = 3$ para bronquiolitis y $\mu_3 = 5$ para influenza, pero no se explicita el cálculo o fuente clínica de dicha escala ordinal.
- **Consecuencia:** Una persona atendida por influenza equivale matemáticamente a cinco por dengue; esa relación exige una fundamentación clínica y epidemiológica sólida.

### Lista de verificación
- [ ] Sustentar la escala de ponderación $\mu_i$ en indicadores clínicos objetivos (tasas de letalidad histórica o riesgo pediátrico/geriátrico OMS/OPS).
- [ ] Documentar el procedimiento de normalización de la escala $\mu_i$.
- [ ] Incluir un análisis de sensibilidad sobre la variabilidad de $\mu_i$ y su impacto en la jerarquización de atención.

---

## 6. Piso Mínimo del 10 %

- **Aspecto no definido:** La planilla exige atender al menos el 10 % de la demanda de cada patología, pero ese porcentaje está escrito directamente en las fórmulas de cálculo y no aparece formalizado en el modelo simbólico del informe.
- **Consecuencia:** Es una decisión sanitaria determinante (especialmente para bronquiolitis), pero sin el debido sustento formal parece un parámetro arbitrario.

### Lista de verificación
- [ ] Formalizar e incorporar explícitamente la restricción de equidad $X_i \ge 0,10 \cdot D_i \quad \forall i$ en el modelo simbólico de LaTeX.
- [ ] Justificar epidemiológicamente el umbral del 10 % como un criterio de resguardo ético-asistencial mínimo innegociable.
- [ ] Analizar y documentar el impacto en la asignación al remover o alterar este piso mínimo.

---

## 7. Función Financiera (Meta 2)

- **Aspecto no definido:** La segunda etapa minimiza únicamente $d_2^+$, es decir, el exceso sobre el presupuesto contingencial de $\$17.000.000$.
- **Consecuencia:** Si el costo total estuviera por debajo de $\$17.000.000$, cualquier solución factible tendría $d_2^+ = 0$, imposibilitando seleccionar la opción de menor costo real.

### Lista de verificación
- [ ] Explicar detalladamente en la metodología el funcionamiento de la Meta 2 bajo escenarios con y sin sobrecosto.
- [ ] Discutir la alternativa de minimizar el costo total real $\sum \sum C_r V_{r,i}$ en lugar de limitarse a la variable de desvío excedente.
- [ ] Demostrar que en el escenario de pico epidemiológico analizado ($SE_{19}$ / $SE_{17}$) la demanda siempre supera el presupuesto, haciendo operativo a $d_2^+$.

---

## 8. Naturaleza de los Costos

- **Aspecto no definido:** Las camas, horas normales de médicos y horas normales de enfermería se cobran en la Meta 2 como si fueran costos extraordinarios variables contingenciales.
- **Consecuencia:** Puede sobreestimarse el fondo de contingencia necesario, porque parte de esos recursos ya forma parte de la estructura fiduciaria y presupuestaria habitual del hospital.

### Lista de verificación
- [ ] Clasificar de forma precisa los recursos entre costos fijos/habituales y costos marginales/extraordinarios de contingencia.
- [ ] Ajustar o justificar la asignación de los valores $C_r$ distinguiendo insumos consumibles de capital humano y físico preexistente.
- [ ] Desglosar en los resultados la diferencia entre costo operativo total y necesidad neta de financiamiento extraordinario.

---

## 9. Carga Base ($B_r$)

- **Aspecto no definido:** Se aplica uniformemente una ocupación previa aproximada del 70 % a prácticamente todos los recursos ($B_r = 0,70 \cdot K_{\text{max},r}$).
- **Consecuencia:** La ocupación de camas, kits, oxígeno y personal difícilmente sea idéntica y constante durante todas las semanas epidemiológicas.

### Lista de verificación
- [ ] Diferenciar los porcentajes de carga base $B_r$ según la naturaleza específica de cada recurso.
- [ ] Documentar la fuente institucional o estadística del SIPROSA para las tasas de ocupación basal habitual.
- [ ] Realizar un análisis de sensibilidad sobre variaciones en la carga base (ej. 60 %, 70 %, 80 %).

---

## 10. Temporalidad (Modelo Estático)

- **Aspecto no definido:** El modelo representa una sola semana epidemiológica aislada.
- **Consecuencia:** No considera pacientes que ingresaron la semana anterior, la duración real de las internaciones (días de estancia), el inventario remanente ni compras futuras.

### Lista de verificación
- [ ] Declarar explícitamente en el marco metodológico y limitaciones que el modelo es estático monoperíodo.
- [ ] Explicar cómo la "Tabla de Uso" permite desacoplar y evaluar semanas críticas individuales.
- [ ] Proponer la formulación de una extensión dinámica multiperiodo como trabajo futuro.

---

## 11. Incertidumbre en la Demanda

- **Aspecto no definido:** La demanda pronosticada por el modelo Holt-Winters ingresa a la programación por metas como un número exacto determinístico ($D_i$).
- **Consecuencia:** El modelo produce una asignación óptima para un pronóstico puntual que puede presentar desviaciones en la práctica.

### Lista de verificación
- [ ] Explicitación del enfoque determinístico del modelo de optimización.
- [ ] Incorporar los márgenes de error del pronóstico ($EA_t$, $MAD$) para construir escenarios de demanda (pesimista, esperado, optimista).
- [ ] Evaluar la sensibilidad del plan de asignación de recursos ante variaciones en la demanda proyectada.

---

## 12. Destino de los No Atendidos ($F_i$)

- **Aspecto no definido:** La variable $F_i$ reúne en una misma categoría a todos los pacientes no atendidos por saturación de capacidad.
- **Consecuencia:** No diferencia entre derivación a la red secundaria/privada, atención ambulatoria diferida, lista de espera o ausencia total de asistencia.

### Lista de verificación
- [ ] Definir conceptualmente el significado operativo y sanitario de $F_i$ en el contexto del SIPROSA (derivación institucional / reprogramación).
- [ ] Categorizar las vías de resolución asistencial para la demanda no absorbida por el efector principal.
- [ ] Incorporar dicha interpretación en la discusión de resultados y conclusiones.

---

## 13. Unicidad de Variables de Recursos ($V_{r,i}$) en Etapa 1

- **Aspecto no definido:** En la primera etapa del algoritmo lexicográfico, como no se minimizan los costos de los recursos, Solver puede asignar cantidades de $V_{r,i}$ superiores a las estrictamente necesarias por protocolo.
- **Consecuencia:** Los valores de asignación de recursos y costos obtenidos al finalizar la Etapa 1 no son únicos ni económicamente interpretables.

### Lista de verificación
- [ ] Incluir una advertencia técnica indicando que los valores de $V_{r,i}$ de la Etapa 1 son instrumentales y no deben reportarse como solución final.
- [ ] Confirmar que los reportes de dotación de recursos y presupuesto provienen de la Etapa 2.
- [ ] Verificar matemáticamente que la minimización de $d_2^+$ en la Etapa 2 ajusta $V_{r,i}$ exactamente al mínimo requerido por protocolo ($T_{i,r} X_i$).

---

## 14. Penalización Social ($P_i$)

- **Aspecto no definido:** La planilla incluye valores de penalización social monetaria ($P_i = \$2.000.000$ para Dengue, $\$5.000.000$ para Bronquiolitis y $\$4.000.000$ para Influenza), pero estos no se utilizan en ninguna función objetivo del modelo.
- **Consecuencia:** Es un parámetro sin efecto sobre la solución matemática que genera dudas sobre su finalidad en la planilla.

### Lista de verificación
- [ ] Aclarar en el informe el rol puramente informativo y de reporte post-hoc de la penalización $P_i$ (cálculo del Costo Social Total $\sum P_i F_i$).
- [ ] Fundamentar por qué $P_i$ se excluye de la función objetivo para preservar la independencia ética de la Meta 1.
- [ ] Incluir el cálculo del Costo Social en la tabla de resultados e interpretación gerencial.
