<template>
    <div id="app">
        <div class="container">
            <h1>Gerador de QR Code para Pix</h1>
            <input
                type="text"
                v-model="pixKey"
                placeholder="Chave Pix"
            />
            <input
                type="number"
                v-model="amount"
                placeholder="Valor (R$)"
            />
            <button @click="generateQRCode">Gerar QR Code</button>
            <div v-if="qrCodeUrl" id="qrCode">
                <img :src="qrCodeUrl" alt="QR Code" />
            </div>
        </div>
    </div>
</template>

<script>
    import { QrCodePix } from 'qrcode-pix';

    export default {
        data() {
            return {
                pixKey: "",
                amount: null,
                qrCodeUrl: null,
            };
        },
        methods: {
            async generateQRCode() {
                if (!this.pixKey || !this.amount) {
                    alert("Por favor, preencha todos os campos.");
                    return;
                }

                try {
                    const qrCode = QrCodePix({
                        version: '01',
                        key: this.pixKey,
                        transactionId: '***',
                        message: 'QR Code PIX',
                        value: this.amount
                    });

                    this.qrCodeUrl = await qrCode.base64();
                } catch (error) {
                    console.error('Erro ao gerar QR Code:', error);
                    alert('Erro ao gerar o QR Code.');
                }
            }
        }
    };
</script>

<style>
body {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
    margin: 0;
    padding: 0;
}

.container {
    max-width: 600px;
    margin: 50px auto;
    padding: 20px;
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    text-align: center;
}

h1 {
    margin-bottom: 20px;
}

input[type="text"], input[type="number"] {
    width: 80%;
    padding: 10px;
    margin: 10px 0;
    border: 1px solid #ccc;
    border-radius: 5px;
}

button {
    padding: 10px 20px;
    border: none;
    background-color: #4CAF50;
    color: white;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #45a049;
}

#qrCode {
    margin-top: 20px;
}
</style>
