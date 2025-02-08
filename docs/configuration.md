from flask import Flask, render_template
from flask_admin import Admin
from flask_admin.contrib.sqla import ModelView
from models import db, Product
from routes.cart import cart_bp  # Import the cart blueprint

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///database.db'
app.config['SECRET_KEY'] = 'your_secret_key'
db.init_app(app)

# Initialize Flask-Admin
admin = Admin(app, name='Admin Panel', template_mode='bootstrap3')
admin.add_view(ModelView(Product, db.session))

# Register the cart blueprint
app.register_blueprint(cart_bp)

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/gallery')
def gallery():
    return render_template('gallery.html')

if __name__ == '__main__':
    with app.app_context():
        db.create_all()
    app.run(debug=True)
    
