flutter create notas_escolares
cd notas_escolares
import 'package:flutter/material.dart';

void main() {
  runApp(NotasEscolaresApp());
}

class NotasEscolaresApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Notas Escolares',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: NotasPage(),
    );
  }
}

class NotasPage extends StatefulWidget {
  @override
  _NotasPageState createState() => _NotasPageState();
}

class _NotasPageState extends State<NotasPage> {
  final _formKey = GlobalKey<FormState>();
  final List<double> _notas = [];
  final _notaController = TextEditingController();

  void _adicionarNota() {
    if (_formKey.currentState!.validate()) {
      setState(() {
        _notas.add(double.parse(_notaController.text));
        _notaController.clear();
      });
    }
  }

  double _calcularMedia() {
    if (_notas.isEmpty) return 0;
    return _notas.reduce((a, b) => a + b) / _notas.length;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Notas Escolares'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            Form(
              key: _formKey,
              child: Row(
                children: [
                  Expanded(
                    child: TextFormField(
                      controller: _notaController,
                      keyboardType: TextInputType.number,
                      decoration: InputDecoration(
                        labelText: 'Digite a nota',
                        border: OutlineInputBorder(),
                      ),
                      validator: (value) {
                        if (value == null || value.isEmpty) {
                          return 'Por favor, insira uma nota';
                        }
                        if (double.tryParse(value) == null) {
                          return 'Digite um número válido';
                        }
                        return null;
                      },
                    ),
                  ),
                  SizedBox(width: 10),
                  ElevatedButton(
                    onPressed: _adicionarNota,
                    child: Text('Adicionar'),
                  ),
                ],
              ),
            ),
            SizedBox(height: 20),
            Text(
              'Média: ${_calcularMedia().toStringAsFixed(2)}',
              style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 20),
            Expanded(
              child: ListView.builder(
                itemCount: _notas.length,
                itemBuilder: (context, index) {
                  return ListTile(
                    title: Text('Nota ${index + 1}: ${_notas[index]}'),
                  );
                },
              ),
            ),
          ],
        ),
      ),
    );
  }
}
flutter run
