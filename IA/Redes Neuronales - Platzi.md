> las redes neuronales son un modelo computacional inspirado en el funcionamiento del cerebro humano. En términos sencillos, constan de capas de nodos (neuronas) que procesan información y aprenden patrones.

Python:

- Es un lenguaje de programación versátil y ampliamente utilizado en ciencia de datos.

Keras:

- Una interfaz de alto nivel para construir redes neuronales, que se ejecuta sobre TensorFlow o Theano.

#### Conceptos clave:

Neurona:

- Unidades básicas de procesamiento, que toman entradas, aplican pesos y bias, y producen una salida.

Capa:

- Conjunto de neuronas que realizan operaciones específicas. Red Neuronal: Conexión de capas que forman el modelo completo.


Las herramientas más conocidas para manejar redes neuronalnes son TensorFlow y PyTorch. _ Keras es una API, se utiliza para facilitar el consumo del backend. _ Utilizaremos la tarjeta GPU, porque permite procesas más datos matemáticos necesarios en el deep learning.

- TENSOR FLOW: BACKEND
- PYTORCH: BACKEND
- KERAS: NO ES UN BACKEND, ES UN API. ESTA HECHO PARA FACILITAR EL CONSUMO DE UN BACKEND. LO UTILIZAREMOS PARA CONECTAR CON TENSORFLOW Y ESTE UTILIZARA GPU (PARTE DE LA CPU QUE PROCESA DATOS A GRAN ESCALA EFICIENTEMENTE)

---
La **inteligencia artificial** son los intentos de replicar la inteligencia humana en sistemas artificiales.

**Machine learning** son las técnicas de aprendizaje automático, en donde mismo sistema aprende como encontrar una respuesta sin que alguien lo este programando.

**Deep learning** es todo lo relacionado a las redes neuronales. Se llama aprendizaje profundo porque a mayor capas conectadas ente sí se obtiene un aprendizaje más fino. __ En el Deep learning existen dos grandes problemas:

- **Overfitting:** Es cuando el algoritmo "memoriza" los datos y la red neuronal no sabe generalizar.
- **Caja negra:** Nosotros conocemos las entradas a las redes neuronales. Sim embargo, no conocemos que es lo que pasa dentro de las capas intermedias de la red.

![[Pasted image 20240508123421.png]]

# Red Neuronal con Keras

