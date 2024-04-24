# back(flaskS)
## setup
1. 新しいプロジェクトディレクトリの作成:
    ```
    mkdir my-flask-backend
    ```
1. 仮想環境の作成
    ```
    cd my-flask-backend
    python3 -m venv .venv
    # linux/Mac
    source .venv/bin/activate
    # windows
    .venv\Scripts\activate
    ```
1. requirements.txtの作成
    ```
    flask
    gunicorn
    ```
1. 必要なパッケージのインストール:
    ```
    pip install -r requirements.txt
    ```
## code
1. ディクレトリ構造
    ```
    /my-flask-app
    │
    ├── app/
    │   ├── __init__.py
    │   ├── models.py
    │   ├── routes.py
    │   └── utilities/      # ユーティリティ関数やクラスを格納するディレクトリ
    │       ├── __init__.py
    │       └── helpers.py  # ユーティリティ関数を含むファイル
    │
    ├── tests/
    │   ├── __init__.py
    │   └── test_routes.py
    │
    ├── .venv/
    │
    ├── pytest.ini
    │
    ├── requirements.txt
    │
    └── run.py

    ```
1. __init__.py
    ```python
    from flask import Flask

    def create_app():
        app = Flask(__name__)

        from .routes import init_routes
        init_routes(app)

        return app
    ```
1. routes.py
    ```py
    def init_routes(app):
        @app.route('/')
        def hello_world():
            return 'Hello, World!'
    ```
1. run.py
    ```py
    from app import create_app

    app = create_app()

    if __name__ == '__main__':
        app.run(debug=True)
    ```
1. Gunicornを使用した実行
    ```
    gunicorn -b 127.0.0.1:8080 "app:create_app()"
    ```
## test
1. pytest.ini
    ```
    [pytest]
    pythonpath = .
    ```
1. test_routes.py
    ```py
    import pytest
    from app import create_app

    @pytest.fixture
    def client():
        app = create_app()
        app.config['TESTING'] = True
        with app.test_client() as client:
            yield client

    def test_hello_world(client):
        response = client.get('/')
        assert response.data == b'Hello, World!'
        assert response.status_code == 200
    ```
1. テストコードの実行
    ```py
    pytest tests
    ```