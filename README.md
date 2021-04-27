# 6 DNA app

1. [Enlaces ](#schema1)
2. [Título ](#schema2)
3. [Entrada de texto](#schema3)
4. [Imprimimos el DNA de entrada](#schema4)
5. [Contar los nucleótidos](#schema5)


<hr>

<a name="schema1"></a>

# 1. Importamos librerías
~~~python
import pandas as pd
import pandas as pd
import streamlit as st
import altair as alt
from PIL import Image
~~~
<hr>

<a name="schema2"></a>

# 2. Título 
Cargamos la imagen.
~~~python
image = Image.open("./images/dna-logo.jpg")
~~~
~~~python
st.image(image, use_column_width = True)

st.write("""
# DNA Nucleotide Count Web App

This app counts the nucleotide composition of query DNA!
***
""")
~~~

<hr>

<a name="schema3"></a>

# 3.Entrada de texto
Texto de entrada.
~~~python

st.header('Enter DNA sequence')
sequence_input = ">DNA Query 2\nGAACACGTGGAGGCAAACAGGAAGGTGAAGAAGAACTTATCCTATCAGGACGGAAGGTCCTGTGCTCGGG\nATCTTCCAGACGTCGCGACTCTAAATTGCCCCCTCTGAGGTCAAGGAACACAAGATGGTTTTGGAAATGC\nTGAACCCGATACATTATAACATCACCAGCATCGTGCCTGAAGCCATGCCTGCTGCCACCATGCCAGTCCT"

sequence = st.text_area("Sequence input", sequence_input, height=250)
sequence = sequence.splitlines()
sequence = sequence[1:] # Quitamos la parte de la sequencia de entrada que pone >DNA Query 2
sequence = ''.join(sequence) # Concatenamos las cadenas

st.write("""
***
""")
~~~
<hr>

<a name="schema4"></a>

# 4. Imprimimos el DNA de entrada
~~~python
st.header('INPUT (DNA Query)')
sequence
~~~

<hr>

<a name="schema5"></a>

# 5. Contar los nucleótidos
~~~python
st.header('OUTPUT (DNA Nucleotide Count)')
~~~


### 1. Imprimomos en forma de diccionario
~~~python
st.subheader('1. Print dictionary')
def DNA_nucleotide_count(seq):
  d = dict([
            ('A',seq.count('A')),
            ('T',seq.count('T')),
            ('G',seq.count('G')),
            ('C',seq.count('C'))
            ])
  return d

X = DNA_nucleotide_count(sequence)

X
~~~

### 2. Imprimomos texto
~~~python
st.subheader('2. Print text')
st.write('There are  ' + str(X['A']) + ' adenine (A)')
st.write('There are  ' + str(X['T']) + ' thymine (T)')
st.write('There are  ' + str(X['G']) + ' guanine (G)')
st.write('There are  ' + str(X['C']) + ' cytosine (C)')
~~~
### 3. Mostramos en forma de DataFrame
~~~python
st.subheader('3. Display DataFrame')
df = pd.DataFrame.from_dict(X, orient='index')
df = df.rename({0: 'count'}, axis='columns')
df.reset_index(inplace=True)
df = df.rename(columns = {'index':'nucleotide'})
st.write(df)
~~~

### 4. Mostramos una visualización de barras
~~~python
st.subheader('4. Display Bar chart')
p = alt.Chart(df).mark_bar().encode(
    x='nucleotide',
    y='count'
)
p = p.properties(
    width=alt.Step(80)  # controls width of bar.
)
st.write(p)