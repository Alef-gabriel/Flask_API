from flask import Flask, request, jsonify, json
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate
from flask_marshmallow import Marshmallow

#configs imports
app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///test.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
app.config['SQLALCHEMY_ECHO'] = True
db = SQLAlchemy(app)
migrate = Migrate(app, db)
ma = Marshmallow(app)

#creation of the database
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50))
    password = db.Column(db.String(50))

#defining serializing
class UserSchema(ma.SQLAlchemySchema):
    class Meta:
        model = User

    id = ma.auto_field()
    name = ma.auto_field()
    password = ma.auto_field()

#get all users
@app.route('/user', methods=['GET'])
def get_all_user():
    bs = UserSchema(many=True)

    users = User.query.all()

    return bs.jsonify(users),200
  
#get one users
@app.route('/user/<name>', methods=['GET'])
def get_one_user(name):
    bs = UserSchema(many=True)

    one = User.query.filter(User.name == name)

    return bs.jsonify(one),200

#create users
@app.route('/user', methods=['POST'])
def create_user():

    data = request.get_json()

    new_user = User(name=data['name'], password=data['password'])
    db.session.add(new_user)
    db.session.commit()

    return jsonify({'message': 'new user created!'})

#changes users
@app.route('/user/<int:id>', methods=['PUT'])
def promote_user(id):

    user= User.query.filter(User.id == id)
    user.update(request.json)

    if not user:
        return jsonify({'USER':'NOT FAUND!!'})

    db.session.commit()

    return jsonify({'USER':'MODIFIQUED'})

#delete users
@app.route('/user/<name>', methods=['DELETE'])
def delete_user(name):

    User.query.filter(User.name == name).delete()

    db.session.commit()

    return jsonify({'DELETED':'SUSSES!!!'})

