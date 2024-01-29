const OpenAI = require('openai').default;
const { ChatOpenAI } = require('@langchain/openai');
const openAIApiKey = "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";
const chatModel = new ChatOpenAI({
    openAIApiKey,
    model: "gpt-4-1106-preview"
});

let clientProfile = {
    "Idade": "22",
    "Peso": "68",
    "Altura": "172cm",
    "Sexo": "Masculino",
    "Objetivo": "Hipertrofia",
    "Circunferencia Cintura": "Não Informado",
    "Circunferencia Quadril": "Não Informado",
    "Circunferencia Antebraco": "Não Informado",
    "Preferencia Alimentar": "Leite integral, Arroz Branco, Frango",
    "Restrições Alimentares": "Batata Doce, Feijão",
    "Alergias Ou Intolerancias": "Não",
    "Não Pode Faltar No plano": "Doce",
    "Nivel de atividade fisica": "Leve",
    "Trabalha em movimento": "Não",
    "Quantidade de refeições": "5",
    "Suplemento": "Não toma",
    "Gasto Total": "2830 kcal"
};

async function processUserInput() {
    const inputForLangChain = createPromptForGPT();
    const response = await chatModel.invoke(inputForLangChain);
    return response.content;
}

function createPromptForGPT() {
    let clientData = JSON.stringify(clientProfile);
    let inputPrompt = `Ficha do Cliente: ${clientData}\n\n` +
                      `Crie um plano alimentar personalizado com base nas informações fornecidas pelo cliente. Certifique-se de seguir as diretrizes e restrições mencionadas na ficha do cliente.\n\n` +
                      `Por favor, leve em consideração as seguintes diretrizes:\n` +
                      `1. Evite incluir alimentos mencionados nas restrições alimentares do cliente.\n` +
                      `2. Forneça detalhes específicos sobre os alimentos recomendados, incluindo a quantidade em gramas ou mililitros e os macronutrientes.\n` +
                      `3. Explique como as calorias foram distribuídas no plano em relação às necessidades calóricas do cliente.\n` +
                      `4. Certifique-se de incluir os alimentos indicados como "Não pode faltar no plano", mesmo que sejam não saudáveis. A quantidade desses alimentos deve ser restrita a 15% do gasto total de calorias.\n` +
                      `5. Evite adicionar gorduras diretamente (por exemplo, 1 colher de azeite).\n` +
                      `6. Não inclua alimentos que estejam nas restrições do cliente.\n\n` +
                      `Elabore um plano alimentar cuidadoso, com alimentos adequados para diferentes refeições do dia.\n` +
                      'Não esqueça de especificar o macronutriente de cada alimento';

    return inputPrompt;
}

// Chamar a função para processar o prompt
processUserInput().then(response => {
    console.log(response);
});
