# script-whatsapp-web
script js que envia mensagens pelo whatsapp web



async function enviarScript(scriptText) {
  const delay = ms => new Promise(resolve => setTimeout(resolve, ms));
  const lines = scriptText.split(/[\n\t]+/).map(line => line.trim()).filter(line => line);

  const main = document.querySelector("#main");
  const textarea = main?.querySelector(`div[contenteditable="true"]`);

  if (!main || !textarea) throw new Error("❌ Nenhuma conversa aberta.");

  for (const line of lines) {
    console.log("✏️ Enviando:", line);
    textarea.focus();

    // Inserir texto usando evento de 'paste'
    const dt = new DataTransfer();
    dt.setData("text", line);
    const pasteEvent = new ClipboardEvent("paste", { clipboardData: dt, bubbles: true });
    textarea.dispatchEvent(pasteEvent);

    await delay(300); // dá tempo de aparecer o botão de envio

    // Captura o botão com base no novo seletor usado pelo WhatsApp Web
    let sendButton = main.querySelector(`button span[data-icon="send"]`)?.closest("button");

    // Se não encontrar o botão, tenta uma segunda alternativa
    if (!sendButton) {
      sendButton = main.querySelector(`button span[data-icon="wds-ic-send-filled"]`)?.closest("button");
    }

    if (!sendButton) {
      console.error("❌ Botão de envio não encontrado para:", line);
      continue;
    }

    sendButton.click();
    await delay(1000); // intervalo entre mensagens
  }

  return lines.length;
}


enviarScript(`



`).then(e => console.log(`Código finalizado, ${e} mensagens enviadas`)).catch(console.error)
