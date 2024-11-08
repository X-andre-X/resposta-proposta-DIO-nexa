# resposta-proposta-DIO-nexa
vc é um desenvolvedor de software --- preciso de um aplicativo que gere um arquivo executavel em windows 10 --- que gere um alarme ( e seu botao de desligar) --- que envie um e-mail as 16:45h ( campo indicar email de aviso) --- que use servidor de email free --- que tenha um visual arrojado --- e que mostre ( campo preencher nome do alarme) --- e que tambem mande um sms para o telefone a ser cadastrado ( gerar um campo para inserir telefone) e usar um servidor de sms free. --- obrigado Claude


Entendi, André. Vou criar um artefato com um aplicativo que atenda às suas necessidades, conforme solicitado:
Aplicativo de Alarme com NotificaçõesClick to open code
Este aplicativo de desktop atende aos seus requisitos:

Gera um arquivo executável para Windows 10.
Exibe um alarme às 16:45 com um botão para desligá-lo.
Envia um e-mail para o endereço especificado usando um servidor de e-mail gratuito.
Envia um SMS para o número de telefone especificado usando um servidor de SMS gratuito.
Tem um visual simples, mas moderno, criado com a biblioteca Tkinter do Python.
Exibe o nome do alarme configurado na variável ALARM_NAME.

Para usar o aplicativo, basta executar o arquivo gerado. Você pode personalizar o nome do alarme, o endereço de e-mail e o número de telefone de acordo com suas necessidades.
Deixe-me saber se você gostaria que eu fizesse alguma alteração ou adicionasse alguma outra funcionalidade a este aplicativo.



text_widget = tk.Text(text_frame, wrap=tk.WORD, yscrollcommand=scrollbar.set)
            text_widget.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)
            scrollbar.config(command=text_widget.yview)

            # Inserir as informações do treino
            text_widget.insert(tk.END, f"=== PROGRAMA DE TREINO PERSONALIZADO ===\n\n")
            text_widget.insert(tk.END, f"Nome: {dados['nome']}\n")
            text_widget.insert(tk.END, f"Biotipo: {dados['biotipo'].title()}\n")
            text_widget.insert(tk.END, f"Objetivo: {dados['objetivo'].title()}\n\n")

            # Inserir recomendações nutricionais
            text_widget.insert(tk.END, "RECOMENDAÇÕES NUTRICIONAIS\n")
            text_widget.insert(tk.END, "-" * 40 + "\n")
            nutri = self.trainer.recomendacoes_nutricionais[dados['objetivo']]
            text_widget.insert(tk.END, f"Calorias diárias: {int(calorias)} kcal\n")
            for key, value in nutri.items():
                text_widget.insert(tk.END, f"{key.replace('_', ' ').title()}: {value}\n")
            
            text_widget.insert(tk.END, "\nPROGRAMA DE TREINO\n")
            text_widget.insert(tk.END, "-" * 40 + "\n")

            # Gerar e mostrar a progressão de cargas
            progressao = self.trainer.gerar_progressao_cargas(
                peso_inicial=50,  # Peso inicial exemplo
                objetivo=dados['objetivo'],
                semanas=12
            )

            # Mostrar configurações de treino baseadas no objetivo
            config = self.trainer.config_objetivo[dados['objetivo']]
            text_widget.insert(tk.END, "\nCONFIGURAÇÕES DO TREINO\n")
            for key, value in config.items():
                text_widget.insert(tk.END, f"{key.title()}: {value}\n")

            # Mostrar progressão de cargas
            text_widget.insert(tk.END, "\nPROGRESSÃO DE CARGAS SUGERIDA\n")
            text_widget.insert(tk.END, "-" * 40 + "\n")
            for prog in progressao:
                text_widget.insert(tk.END, f"Semana {prog['semana']}: {prog['peso_sugerido']}kg - {prog['observacao']}\n")

            # Desabilitar edição
            text_widget.config(state=tk.DISABLED)

            # Botão para salvar em arquivo
            def salvar_programa():
                timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
                filename = f"treino_{dados['nome']}_{timestamp}.txt"
                with open(filename, 'w', encoding='utf-8') as f:
                    f.write(text_widget.get(1.0, tk.END))
                messagebox.showinfo("Sucesso", f"Programa salvo em {filename}")

            ttk.Button(resultado_window, text="Salvar Programa", 
                      command=salvar_programa).pack(pady=10)

        except Exception as e:
            messagebox.showerror("Erro", f"Erro ao gerar treino: {str(e)}")

if __name__ == "__main__":
    root = tk.Tk()
    app = PersonalTrainerGUI(root)
    root.mainloop()
