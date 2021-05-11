**UNIVERSIDADE LUSÓFONA DE HUMANIDADES E TECNOLOGIAS**

# Programação Web - Laboratório 6: django web app 🔥

**OBJECTIVO**: 
* Neste laboratório criará uma primeira aplicação django simples, para se familiarizar com os conceitos de urls, views, templates e sua linguagem. 
* O tópico e conteúdos do website podem ser extraídos se quiser do seu projeto ou lab5, mas não precisam ser muitos conteúdos. A ideia é trabalhar a estrutura e dinamicidade. 
* Exercitará a edição dos módulos urls.py, views.py e a criação de templates HTML com linguagem template.

**RECOMENDAÇÕES**: 
* Instale e use o Pycharm para editar o código de forma fácil.
* * quando necessário, guie-se pelo projeto que fizemos na aula teórica, que  está disponível no [repo GitHub](https://github.com/ULHT-PW-2020-21/pw-django-01). 
* se tiver dúvidas, consulte os [slides](https://secure.grupolusofona.pt/ulht/moodle/pluginfile.php/800079/course/section/398731/pw-03-django-01.pptx) e a documentação do [djangoproject](https://www.djangoproject.com/)

## 1. Primeiros passos 👶
### 1.1. Crie um projeto e app django
1. Abra a linha de comandos (PowerShell ou cmd)
1. Crie e entre na pasta lab6 `mkdir lab6; cd lab6`
1. Crie um ambiente virtual com django `pipenv install django`
1. Active o ambiente virtual `pipenv shell`
1. crie um projeto django `django-admin startproject config .`
1. Migre as base de dados `python manage.py migrate`
1. Lance o projeto para ver se está tudo ok, com o comando `python manage.py runserver` 
1. Pare o servidor com Ctrl + C
1. Crie a aplicação website, com a instrução `python manage.py startapp website`

### 1.2. Configure a aplicação
1. abra a pasta com o Pycharm
1. em config\settings.py registe a aplicação na lista INSTALLED_APPS, colocando no fim `'website'`
1. em config\urls.py registe a rota para a nova aplicação website, inserindo na lista urlpatterns o caminho `path('', include('website.urls))` para a sua aplicação, ficando:

```python
# config\urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('website.urls')),
]
```

## 3. Templates 🖺

### 3.1 Layout
1. na pasta `website` crie a pasta `templates`, e dentro dessa a pasta `\website`, ficando com o caminho `lab6\website\templates\website`
1. Crie, na pasta `website\templates\website`, o ficheiro `base.html`, usando o snippet HTML5 sugerido pelo Pycharm. 
1. integre no elemento `head` um link para o bootstrap, `<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">`. 
2. O layout será inspirado no Bootstrap [jumbotron](https://www.w3schools.com/bootstrap4/bootstrap_jumbotron.asp) e detalha-se de seguida
 
#### header
1. No body crie o elemento `<header class="jumbotron text-center">` que evidenciará o cabeçalho do website. Dentro do elemento header deverá ter aninhado três elementos:
    1. um elemento `<h1>`com o título do website
    1. um elemento `<p>` com uma frase curta da mesma largura do título
    2. um elemento `<nav>` três hiperlinks `<a>` para três páginas a criar, cada com a classe `class="btn btn-info"` que transforma o hiperlink num botão (ficando por exemplo `<a href="" class="btn btn-info">Home</a>)`

#### main
1. No body crie uma secção `<article class="container">`. O article irá ter dentro dois elementos, o `<main>` e o `<aside>`.
1. O elemento `main` tem duas classes bootstrap e um bloco que será estendido com os conteúdos das páginas do wesite. 
```html
<main class="col-sm-6"> 
	{% block main %}
	{% endblock main %}
</main>
```
1. O elemento `<aside>` com a classe bootstrap que ocupará 6 colunas de largura ([responsive grid](https://www.w3schools.com/css/css_rwd_grid.asp)), `<aside class="col-sm-6">`. 
1. Dentro deste elemento irá inserir um elemento `<img>`, sendo os detalhes dados na secção seguinte.

#### footer
1. crie um elemento `<footer></footer>` com um texto simples de rodapé. 

### 3.2 HTML Templates 

Crie três templates HTML que estendam o layout base.html. Estes terão os conteúdos que irão aparecer no elemento main. Deverão ser de texto, duas ou tres frases basta.

```html
{% extends 'website/base.html' %}

{% block main %}
    <h3>Titulo</h3>
    <p>texto texto texto texto texto texto texto </p>
{% endblock %}
```

## 4. Static 🖼️
1. Os ficheiros estáticos são imagens, CSS e scripts JavaScript
2. na pasta `website` crie a pasta `static`, e dentro dessa a pasta `website`. Esta pasta conterá uma pasta para as imagens e outra para o CSS. 

### 4.1 CSS
1. Crie dentro de `website\static\website` a pasta `css` (ficando com o caminho `lab6\website\static\website\css` 😱), incluindo nesta o ficheiro `base.css`.
1. configure neste a estilização do elemento footer, por forma a que fique em baixo. Poderá configurar desta forma:  
```html
footer {
   position: fixed;
   bottom: 30px;
   width: 100%;
   text-align: center;
}
```
1. configure o elemento article de forma a ficar centrado e com largura máxima de 800px
```html
body > article {
    max-width: 800px;
    margin: auto;
}
```

### 4.2 images
1. Crie em `website\static\website` a pasta `images` (ficando com o caminho `lab6\website\static\website\images` 😱).
2. Inclua uma imagem a seu gosto, com uma largura máxima de 200px, que irá ficar no elemento aside acima definido.

## 5. Views ⚙️
1. no ficheiro `views.py` crie uma função view que renderize cada uma das páginas. Por exemplo, para renderizar a página home.html teremos a função `home_page_view`:

```python
#  hello/views.py

from django.http import render

def home_page_view(request):
	return render(request, 'website\home.html')
```

## 6. URLS ✉️
1. Na pasta website crie o ficheiro `urls.py`:
```python
#  hello/urls.py

from django.shortcuts import render
from . import views

app_name = "website"

urlpatterns = [
    path('home', views.home_page_view, name='home')
]
```
Este irá importar o módulo views que se encontra na mesma pasta (e por isso é importado como `from . import views`). Será igualmente importada a função render. Na lista urlpatterns deverá incluir uma rota para cada uma das três views anteriormente criadas. 


# 7. Ready, go! 🏁
1. Lance a aplicação com o comando `python manage.py runserver` e verifique que consegue visualizar corretamente a aplicação que fez. 

