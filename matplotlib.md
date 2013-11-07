~~~ headers
input:         theme.tex
usepackage:     graphicx, wasysym, hyperref 
usepackage:     color
usetheme:       CambridgeUS
usecolortheme:  orchid

title:          [Matplotlib]
author:         [BA] Benjamin Audren
institute:      [EPFL] \'Ecole Polytechnique F\'ed\'erale de Lausanne
date:           \today

outline-at-sections
no-navigation-symbols
~~~

=================================
Nanny tales about Matplotlib
=================================

# Title #

Idea
----

  ~~~ block | Motivation; 80%, <1->
  Last week, you saw most of what {\Red\bf Matplotlib} is capable of
  doing. But when confronted with it alone, one can get lost with its
  {\Green\bf documentation}, and they way the {\Blue\bf examples} are
  written. 
  ~~~

  ~~~ alertblock | First Goal; 80%, <2->
  To provide you with a {\Red\bf basic understanding} of the main
  differences between using the {\Green\bf machine state} inside
  {\Blue\bf pyplot} and explicitely accessing the objects through their
  reference. 
  ~~~

  ~~~ exampleblock | Second Goal; 80%, <3>
  Ensure that the {\Red\bf vocabulary} is written once and for all,
  and understood.
  ~~~

# Outline #

# Understanding the (many) matplotlib.pyplot functions

## Forewords


Foreword on interactive mode
----------------------------

  ~~~ block | It is not much, really; 60%
  Simply prevent you from using \pythoninline{plt.show()}
  ~~~

Foreword on module names
------------------------

  ~~~ block | Pylab and Pyplot; 80%, <1->
  {\Red\bf Matplotlib} is the parent module, that contains two
  submodules. 
  * {\Green\bf Pyplot} contains simply the plotting tool, with its
    machine state (later), etc... 
  * {\Blue\bf Pylab} wraps in {\bf Pyplot and Numpy} for convenience.

  ~~~

  ~~~ alertblock | Launchin ipython with pylab; 60%
  This does \pythoninline{from matplotlib.pylab import  }
  ~~~


Understanding the documentation
-------------------------------

\url{http://matplotlib.org/api/pyplot_summary.html}

You will find here a complete list of all the functions inside pyplot.
{\Red\bf There are major differences between them !}, though they are
presented at the same level...


On the many ways of using Pyplot
--------------------------------

  ~~~ python 
  import matplotlib.pyplot as plt
  import numpy as np

  x = np.linspace(0, 10, 100)
  ~~~

  ~~~ columns

  ~~~ column; 45% ~~~

    machine state
    \pythonstyle
    \lstset{basicstyle=\ttfamily\scriptsize,breaklines = true,
language=Python}
    \begin{lstlisting}
    plt.plot(np.cos(x))

    plt.show()
    \end{lstlisting}

  ~~~ column; 45% ~~~
    standard
    \pythonstyle
    \lstset{basicstyle=\ttfamily\scriptsize,breaklines = true,
language=Python}

    \begin{lstlisting}
    fig = plt.figure()
    ax = fig.add_subplot()

    ax1.plot(np.cos(x))

    plt.show()
    \end{lstlisting}

  ~~~

On the many ways of using Pyplot
--------------------------------

  ~~~ python 
  import matplotlib.pyplot as plt
  import numpy as np

  x = np.linspace(0, 10, 100)
  ~~~

  ~~~ columns

  ~~~ column; 45% ~~~

    machine state
    \pythonstyle
    \lstset{basicstyle=\ttfamily\scriptsize,breaklines = true,
language=Python}
    \begin{lstlisting}
    plt.subplot(121)
    plt.plot(np.cos(x))

    plt.subplot(122)
    plt.plot(np.sin(x))

    plt.show()
    \end{lstlisting}

  ~~~ column; 45% ~~~
    standard
    \pythonstyle
    \lstset{basicstyle=\ttfamily\scriptsize,breaklines = true,
language=Python}
    \begin{lstlisting}
    fig = plt.figure()
    ax1 = fig.add_subplot(121)
    ax2 = fig.add_subplot(122)

    ax1.plot(np.cos(x))
    ax2.plot(np.sin(x))

    plt.show()
    \end{lstlisting}

  ~~~

On the many ways of using Pyplot
--------------------------------

  ~~~ columns

  ~~~ column; 45% ~~~

    machine state
    \pythonstyle
    \lstset{basicstyle=\ttfamily\scriptsize,breaklines = true,
language=Python}
    \begin{lstlisting}
    plt.subplot(121)
    plt.plot(np.cos(x))

    plt.subplot(122)
    plt.plot(np.sin(x))

    plt.xlim(0, 10)
    plt.show()
    \end{lstlisting}

  ~~~ column; 45% ~~~
    standard
    \pythonstyle
    \lstset{basicstyle=\ttfamily\scriptsize,breaklines = true,
language=Python}
    \begin{lstlisting}
    fig = plt.figure()
    ax1 = fig.add_subplot(121)
    ax2 = fig.add_subplot(122)

    ax1.plot(np.cos(x))
    ax2.plot(np.sin(x))

    ax2.set_xlim(0, 10)
    plt.show()
    \end{lstlisting}

  ~~~

On the many ways of using Pyplot
--------------------------------

  ~~~ columns

  ~~~ column; 45% ~~~

    machine state
    \pythonstyle
    \lstset{basicstyle=\ttfamily\scriptsize,breaklines = true,
language=Python}
    \begin{lstlisting}
    plt.plot(np.cos(x))

    plt.figure()
    plt.plot(np.sin(x))

    plt.show()
    \end{lstlisting}

  ~~~ column; 45% ~~~
    standard
    \pythonstyle
    \lstset{basicstyle=\ttfamily\scriptsize,breaklines = true,
language=Python}
    \begin{lstlisting}
    fig1 = plt.figure()
    ax1 = fig1.add_subplot()

    ax1.plot(np.cos(x))

    fig2 = plt.figure()
    ax2 = fig2.add_subplot()
    ax2.plot(np.sin(x))

    plt.show()
    \end{lstlisting}

  ~~~


How does this state machine work ?
----------------------------------

\pythonstyle
\pythoninline{plt.plot(x)} does different things depending on the
context:

* it searches for the current figure (\pythoninline{plt.gcf()}), and
  create one if absent
* it adds an axes object to this current figure
  (\pythoninline{plt.gcf().add_subplot(111)})
* it plots on this object $x$ (\pythoninline{plt.gca().plot(x)})


General rules
-------------

  ~~~ block | When encountering a script written in a certain way; 80%, <1->
  Go to the documentation, search for the first attribute of this
{\Green\bf machine state function} and search inside this object for a
function called \pythoninline{set_something()}
  ~~~

  ~~~ exampleblock | Example; 80%, <2>
  \url{http://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.xlim}
  It tells you about current axes object, go to axes documentation
  \url{http://matplotlib.org/api/axes_api.html#matplotlib.axes.Axes.set_xlim}
  Et voila !
  ~~~
  

## When Pyplot is not enough

Sometimes, you need better options
-----------------------------------
Matplotlib module

  ~~~ block | matplotlibrc file; 70%, <1->
  contains settings used for {\Red\bf all} your figures
  ~~~

  ~~~ block | When changing options in one script; 70%, <2>
  \pythoninline{import matplotlib as mpl} 
  \pythoninline{mpl.rcParams['lines.linewidth'] = 2}
  \pythoninline{mpl.rcParams['lines.color'] = 'r'}
  \pythoninline{mpl.rcParams['text.usetex'] = True}
  ~~~


# Vocabulary

## Because when you do not know what to search...

Basic objects
-------------

  ~~~ block | Elementary structures; 80%
  * figure (called from pyplot \pythoninline{fig = plt.figure()})
  * axes (attached to a figure \pythoninline{ax =
    fig.add_subplot(111)}
  * axis (attached to an axes instance)
  * spines (attached to an axes instance)
  ~~~

  ~~~ vspace; 1cm ~~~

\url{http://matplotlib.org/api/spines_api.html}


## Advanced

Artists
-------

  ~~~ block | What are they ?; 70%
  Actual guys that go and draw things on the canvas ! There are two
types:
  * {\Red\bf Primitives}: Line2D, Rectangle, Text
  * {\Green\bf Containers}: Axis, Axes, Figure
  ~~~

  ~~~ vspace; 1cm ~~~

  This is a good read \url{http://matplotlib.org/users/artists.html}

