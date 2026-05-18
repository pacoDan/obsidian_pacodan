Respecto a la inecuación de un concentrador ($\sum E_i > C$), si el primer término (la sumatoria de las velocidades de entrada) llega a ser igual al segundo (la capacidad de salida), es decir, **$\sum E_i = C$**, el dispositivo deja de comportarse como un concentrador y pasa a ser técnicamente un **multiplexor**, específicamente de tipo **TDM** (División de Tiempo) convencional,.

Esta transición tiene las siguientes implicancias según las fuentes:

### 1. Reflexión sobre la Calidad del Servicio (QoS)

- **Garantía de disponibilidad:** En este escenario, la calidad del servicio es **óptima y transparente**. A diferencia del concentrador, donde los usuarios deben "esperar" si todos transmiten al mismo tiempo, en el caso de igualdad cada canal tiene su ancho de banda **siempre disponible**.
- **Eliminación de esperas:** Se elimina la penalización de calidad inherente al concentrador. No hay riesgo de pérdida de información ni retrasos por congestión, ya que el enlace de salida tiene la capacidad exacta para soportar la carga máxima simultánea de todas las entradas.

### 2. Reflexión sobre el Ahorro

- **Pérdida de la ventaja económica:** El ahorro es la razón de ser del concentrador. Las fuentes indican que el concentrador es una **variante más económica** precisamente porque utiliza un canal de salida de menor capacidad ($C$) que la suma de sus entradas,.
- **Sobredimensionamiento para fines de ahorro:** Al igualar los términos, se pierde el beneficio del ahorro en infraestructura. El ahorro en un concentrador se basa en la **premisa estadística** de que no todos los abonados usarán el sistema al mismo tiempo (como ocurre en las centrales telefónicas, donde solo hay unidades de discado para un 10% de los usuarios),.
- **Inversión superior:** Si $\sum E_i = C$, se está invirtiendo en un vínculo de salida más robusto (y costoso), lo cual es ideal para alta densidad de tráfico pero contradice la lógica de reducción de costos por la que se optaría por un concentrador en primer lugar,.

En conclusión, si $\sum E_i = C$, se obtiene una **máxima calidad de servicio** sin demoras, pero se **sacrifica el ahorro económico** que justifica el uso de la técnica de concentración,.