from flask import Flask, request, jsonify
import requests

app = Flask(__name__)

# Ganti dengan API KEY kamu sendiri
API_KEY = "RAMA-API-123456"

@app.route("/ai", methods=["GET"])
def akmal_ai():
    key = request.args.get("key")
    question = request.args.get("q")

    # Cek key
    if key != API_KEY:
        return jsonify({"error": "Invalid API Key"}), 403

    if not question:
        return jsonify({"error": "No question provided"}), 400

    try:
        # Contoh pakai API AI (ganti sesuai API kamu)
        response = requests.post(
            "https://api.openai.com/v3/chat/completions",
            headers={
                "Authorization": "Bearer API_OPENAI_KAMU",
                "Content-Type": "application/json"
            },
            json={
                "model": "gpt-4o-mini",
                "messages": [{"role": "user", "content": question}]
            }
        )

        data = response.json()
        answer = data["choices"][0]["message"]["content"]

        return jsonify({
            "creator": "Akmal",
            "question": question,
            "answer": answer
        })

    except Exception as e:
        return jsonify({"error": str(e)})

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
