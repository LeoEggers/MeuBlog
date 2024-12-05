
# Criando Seu Primeiro Blog com Ruby on Rails

Tutorial de instalação do Ruby, Rails, dependências necessárias e criação do seu primeiro blog em uma plataforma Ubuntu. Baseado nas instruções de **Eri A. Assunção**, o post original pode ser acessado aqui: [https://github.com/EriAssunPereira/Crie-Seu-Pr-prio-Blog-com-Ruby-On-Rails](https://github.com/EriAssunPereira/Crie-Seu-Pr-prio-Blog-com-Ruby-On-Rails)

---

## 🖥 **Instalação**

### 1. Instale o Ruby e o Rails
Certifique-se de ter o Ruby, o Rails, o Git e o Rake instalados:

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install ruby -y
sudo apt install ruby-dev build-essential -y
sudo gem install rails bundler
sudo apt install git
sudo gem install rake
```

Verifique a instalação:

```bash
ruby -v    # Saída: Ruby 3.x.x
rails -v   # Saída: Rails 7.x.x
git --version # Saída: git version x.x.x
rake -V    # Saída: rake, version x.x.x
```

### 2. Crie o projeto:

```bash
rails new MeuBlog
```

### 3. Instale as dependências DENTRO DA PASTA DO PROJETO:

```bash
cd MeuBlog
bundle install
```

### 4. Inicie o servidor:

```bash
rails server
```

Acesse [http://localhost:3000](http://localhost:3000) no navegador para verificar se o Rails está funcionando.

---

## 🏗 **Criando o Modelo**

### 1. Gere o modelo para os posts:

```bash
rails generate model Post title:string content:text
```

### 2. Execute a migração para criar a tabela no banco de dados:

```bash
rails db:migrate
```

---

## 🔗 **Configurando Rotas**

Abra o arquivo `config/routes.rb` e configure as rotas:

```ruby
Rails.application.routes.draw do
  resources :posts
  root 'posts#index'
end
```

---

## ✍️ **Criando o Controlador e Views**

### 1. Gere o controlador para os posts:

```bash
rails generate controller Posts
```

### 2. Implemente as ações básicas no controlador `app/controllers/posts_controller.rb`:

```ruby
class PostsController < ApplicationController
  def index
    @posts = Post.all
  end

  def show
    @post = Post.find(params[:id])
  end

  def new
    @post = Post.new
  end

  def create
    @post = Post.new(post_params)
    if @post.save
      redirect_to @post, notice: 'Post criado com sucesso!'
    else
      render :new
    end
  end

  private

  def post_params
    params.require(:post).permit(:title, :content)
  end
end
```

### 3. Crie as views básicas em `app/views/posts`:

#### **`index.html.erb` (Lista de posts):**

```erb
<h1>Lista de Posts</h1>

<%= link_to 'Novo Post', new_post_path, class: 'btn btn-primary' %>

<ul>
  <% @posts.each do |post| %>
    <li><%= link_to post.title, post %></li>
  <% end %>
</ul>
```

#### **`show.html.erb` (Detalhes do post):**

```erb
<h1><%= @post.title %></h1>
<p><%= @post.content %></p>

<%= link_to 'Editar', edit_post_path(@post), class: 'btn btn-warning' %>
<%= link_to 'Voltar', posts_path, class: 'btn btn-secondary' %>
```

#### **`new.html.erb` (Novo post):**

```erb
<h1>Novo Post</h1>

<%= form_with(model: @post, local: true) do |form| %>
  <div>
    <%= form.label :title, 'Título' %>
    <%= form.text_field :title %>
  </div>

  <div>
    <%= form.label :content, 'Conteúdo' %>
    <%= form.text_area :content %>
  </div>

  <%= form.submit 'Criar Post', class: 'btn btn-success' %>
<% end %>
```

---

## 🔧 **Testando**

1. Acesse o servidor em [http://localhost:3000](http://localhost:3000).
2. Crie um novo post.
3. Visualize, edite e apague posts.

---

Pronto! Você criou seu primeiro blog com Ruby on Rails. Explore mais funcionalidades e personalize sua aplicação. 🎉
