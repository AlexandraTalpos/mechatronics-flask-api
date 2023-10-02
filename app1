from flask import Flask, request, jsonify, render_template
from flask_sqlalchemy import SQLAlchemy

app1 = Flask(__name__, template_folder='/Users/alle/Desktop/Proiect1/Python') 

# Configure the MySQL database 
app1.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://root:Madalina27@localhost:3306/Database1'
db = SQLAlchemy(app1)

@app1.route('/')
def index():
    return render_template('index.html')


# Define the MechatronicDevice model
class MechatronicDevice(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    description = db.Column(db.String(255))

    def __init__(self, name, description, **kwargs):
        super().__init__(**kwargs)
        self.name = name
        self.description = description

# Create the database tables (uncomment when running the first time)
#db.create_all()

# Routes to manage mechatronic devices
@app1.route('/devices', methods=['POST'])
def create_device():
    data = request.get_json()
    name = data.get('name')
    description = data.get('description')

    if name is None or description is None:
        return jsonify({'error': 'Missing "name" or "description" field'}), 400

    new_device = MechatronicDevice(name=name, description=description)
    db.session.add(new_device)
    db.session.commit()

    return jsonify({'message': 'Device created successfully!'})

@app1.route('/devices', methods=['GET'])
def get_all_devices():
    devices = MechatronicDevice.query.all()
    device_list = [{'id': device.id, 'name': device.name, 'description': device.description} for device in devices]
    return jsonify({'devices': device_list})


@app1.route('/devices/<int:device_id>', methods=['GET', 'PUT', 'DELETE'])
def manage_device(device_id):
    device = MechatronicDevice.query.get_or_404(device_id)

    if request.method == 'GET':
        return jsonify({'id': device.id, 'name': device.name, 'description': device.description})

    elif request.method == 'PUT':
        data = request.get_json()
        device.name = data['name']
        device.description = data['description']
        db.session.commit()
        return jsonify({'message': 'Device updated successfully!'})

    elif request.method == 'DELETE':
        db.session.delete(device)
        db.session.commit()
        return jsonify({'message': 'Device deleted successfully!'})
    
if __name__ == '__main__':
    app1.run(host='localhost', port=5000, debug=True)
