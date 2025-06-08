Interface do App (index.html) — Pasta: frontend/index.html
<!DOCTYPE html>
<html>
<head>
    <title>Auto Trader - Painel</title>
    <style>
        body { background-color: #e6fff2; font-family: Arial, sans-serif; text-align: center; padding: 40px; }
        .button { background-color: #00cc66; color: white; padding: 20px 40px; font-size: 24px; border: none; border-radius: 8px; cursor: pointer; }
        .status { margin-top: 30px; font-size: 20px; color: #333; }
        input, select { padding: 10px; margin: 10px; font-size: 18px; }
    </style>
</head>
<body>
    <h1>Auto Trader</h1>
    <form method="POST" action="/login">
        <input type="email" name="email" placeholder="E-mail" required><br>
        <input type="password" name="password" placeholder="Senha" required><br>
        <select name="percentual">
            <option value="10">Usar 10% da banca</option>
            <option value="20">Usar 20% da banca</option>
            <option value="30">Usar 30% da banca</option>
        </select><br>
        <button class="button" type="submit">INICIAR OPERAÇÕES</button>
    </form>
    <div class="status">
        <p>Status: aguardando início...</p>
    </div>
</body>
</html>
Código do App (app.py) — Pasta: backend/app.py
from flask import Flask, request, redirect, render_template_string

app = Flask(__name__)

@app.route('/', methods=['GET'])
def home():
    return open('frontend/index.html').read()

@app.route('/login', methods=['POST'])
def login():
    email = request.form.get('email')
    password = request.form.get('password')
    percentual = int(request.form.get('percentual'))

    # Simula cálculo de entradas automáticas
    banca = 200  # Simulado
    valor_entrada = (banca * percentual) / 100 / 20
    entradas = [{'ativo': f'Ativo_{i+1}', 'valor': round(valor_entrada, 2)} for i in range(20)]

    log_operacoes = "<br>".join([f"{e['ativo']}: R${e['valor']}" for e in entradas])
    return render_template_string(f'''
        <h1>Operações Iniciadas!</h1>
        <p>Email: {email}</p>
        <p>Usando {percentual}% da banca</p>
        <h3>Entradas simuladas:</h3>
        <p>{log_operacoes}</p>
        <a href="/">Voltar</a>
    ''')

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8000)
Instruções de Uso (README.txt)
# Auto Trading App - Painel Web

## Requisitos:
- Python 3
- Flask (instale com: pip install flask)

## Como rodar:
1. Coloque o `index.html` na pasta `frontend/`
2. Coloque o `app.py` na pasta `backend/`
3. Execute o app:
   cd backend
   python app.py

4. Acesse no navegador:
   http://localhost:8000
   (ou no seu iPhone: http://IP_DA_SUA_VPS:8000)

Este painel simula as operações com base em percentuais da banca e mostra as entradas.
    
