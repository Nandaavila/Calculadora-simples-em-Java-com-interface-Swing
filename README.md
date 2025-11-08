//# Calculadora-simples-em-Java-com-interface-Swing
//Projeto de uma **calculadora simples com interface gráfica (Swing)**, desenvolvida em Java.   
//Permite realizar operações básicas de soma, subtração, multiplicação e divisão

package Menu;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class Calculadora extends JFrame implements ActionListener {

    /**
	 * 
	 */
	private static final long serialVersionUID = 1L;
	private JTextField campoTexto;
    private double num1 = 0, num2 = 0, resultado = 0;
    private char operador;

    public Calculadora() {
        setTitle("Calculadora Simples");
        setSize(320, 420);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout(10, 10));
        getContentPane().setBackground(new Color(240, 240, 240));

        campoTexto = new JTextField();
        campoTexto.setEditable(false);
        campoTexto.setFont(new Font("Arial", Font.BOLD, 28));
        campoTexto.setHorizontalAlignment(JTextField.RIGHT);
        campoTexto.setBackground(Color.WHITE);
        campoTexto.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));
        add(campoTexto, BorderLayout.NORTH);

        JPanel painelBotoes = new JPanel();
        painelBotoes.setLayout(new GridLayout(5, 4, 10, 10));
        painelBotoes.setBackground(new Color(240, 240, 240));
        painelBotoes.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));

        String[] botoes = {
            "C", "", "", "/",
            "7", "8", "9", "*",
            "4", "5", "6", "-",
            "1", "2", "3", "+",
            "0", ".", "=", ""
        };

        for (String texto : botoes) {
            JButton botao = new JButton(texto);
            botao.setFont(new Font("Arial", Font.BOLD, 18));
            botao.setFocusPainted(false);

            if (texto.equals("C")) {
                botao.setBackground(new Color(255, 102, 102));
                botao.setForeground(Color.WHITE);
            } else if (texto.matches("[+\\-*/]")) {
                botao.setBackground(new Color(100, 149, 237));
                botao.setForeground(Color.WHITE);
            } else {
                botao.setBackground(Color.WHITE);
            }

            if (!texto.isEmpty()) { 
                botao.addActionListener(this);
            } else {
                botao.setEnabled(false);
            }

            painelBotoes.add(botao);
        }

        add(painelBotoes, BorderLayout.CENTER);
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        String comando = e.getActionCommand();

        if (comando.equals("C")) {
            campoTexto.setText("");
            num1 = num2 = resultado = 0;
            operador = '\0';
            return;
        }

        if ((comando.charAt(0) >= '0' && comando.charAt(0) <= '9') || comando.equals(".")) {
            campoTexto.setText(campoTexto.getText() + comando);
        } else if (comando.matches("[+\\-*/]")) {
            try {
                num1 = Double.parseDouble(campoTexto.getText());
                operador = comando.charAt(0);
                campoTexto.setText("");
            } catch (Exception ex) {
                campoTexto.setText("Erro");
            }
        } else if (comando.equals("=")) {
            try {
                num2 = Double.parseDouble(campoTexto.getText());
                switch (operador) {
                    case '+': resultado = num1 + num2; break;
                    case '-': resultado = num1 - num2; break;
                    case '*': resultado = num1 * num2; break;
                    case '/': resultado = num2 != 0 ? num1 / num2 : Double.NaN; break;
                }
                campoTexto.setText(String.valueOf(resultado));
            } catch (Exception ex) {
                campoTexto.setText("Erro");
            }
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            Calculadora calc = new Calculadora();
            calc.setVisible(true);
        });
    }
}
