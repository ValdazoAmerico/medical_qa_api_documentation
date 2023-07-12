# Documentación de la API: Asistente Médico

La API del Asistente Médico proporciona asistencia en consultas médicas y genera preguntas, notas médicas, diagnósticos, planes de acción y más, basado en transcripciones de audio.

## Uso

### `POST /medical_qa`

Genera preguntas o notas médicas basadas en la transcripción de audio.

#### Solicitud

- Method: `POST`
- Endpoint: `https://apis.umasalud.com/ai/medical_qa`
- Body:
  - `text`: Cadena de texto con la transcripción de audio para generar preguntas o notas médicas.
  - `step`: Cadena de texto que indica la etapa del procesamiento ("first" o "second").
 
#### Ejemplo de solicitud (first step)

```json
{
"text": "estoy con fiebre"
"step": "first"
}
```

#### Respuesta (first step)

- Cuando `step` se establece en `"first"`, la respuesta será un objeto JSON que contiene las preguntas generadas.

  Ejemplo de respuesta:
  
```json
{
"output": 
"Transcripción: estoy con fiebre\nInterrogatorio: ¿Desde cuándo tienes fiebre?\n¿Cuál es la temperatura de tu fiebre?\n¿Has tenido otros síntomas además de la fiebre?\n¿Has tomado algún medicamento para la fiebre?\n¿Has tenido contacto con alguien que esté enfermo?\n¿Has viajado recientemente?\n¿Has notado algún cambio en tu apetito o en tu peso?\n¿Has tenido alguna enfermedad reciente?"
}
```

#### Ejemplo de solicitud (second step)

```json
{
"text": 
"Transcripción: "estoy con fiebre\nInterrogatorio: ¿Desde cuándo tienes fiebre? Desde ayer\n¿Cuál es la temperatura de tu fiebre? 29\n¿Has tenido otros síntomas además de la fiebre? No\n¿Has tomado algún medicamento para la fiebre? Paracetamol\n¿Has tenido contacto con alguien que esté enfermo? No\n¿Has viajado recientemente? No\n¿Has notado algún cambio en tu apetito o en tu peso? No\n¿Has tenido alguna enfermedad reciente? No"
"step":"second"
}
```

#### Respuesta (second step)

- Cuando step se establece en "second", la respuesta será un objeto JSON que contiene la nota médica, los diagnósticos diferenciales, el plan de acción, los estudios complementarios, las pautas de alarma y el destino.

  Ejemplo de respuesta:
  
```json
{
  "output": {
    "nota_medica_pase_guardia": "Paciente con fiebre desde ayer, temperatura de 29. No presenta otros síntomas. Ha tomado paracetamol. No ha tenido contacto con personas enfermas ni ha viajado recientemente. No ha notado cambios en su apetito o peso. No ha tenido enfermedades recientes.",
    "diagnosticos_diferenciales": [
      "Infección viral",
      "Infección bacteriana",
      "Enfermedad autoinmune"
    ],
    "plan_de_accion_sugerido": "Recomendar al paciente continuar tomando paracetamol para controlar la fiebre. Monitorear la temperatura y si persiste o empeora, buscar atención médica presencial. Mantenerse hidratado y descansar adecuadamente.",
    "estudios_complementarios": "No se requieren estudios complementarios en este momento.",
    "pautas_de_alarma": [
      "Si la fiebre persiste o aumenta después de 48 horas",
      "Si aparecen otros síntomas como dolor de cabeza intenso, dificultad para respirar o dolor abdominal",
      "Si la temperatura supera los 39 grados Celsius"
    ],
    "destino_final": "En domicilio con instrucciones"
  }
}
```
