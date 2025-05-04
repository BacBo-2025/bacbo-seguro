import React, { useState, useEffect } from 'react';
import { View, Text, StyleSheet, Button, ScrollView } from 'react-native';

function rolarDado() {
  return Math.floor(Math.random() * 6) + 1;
}

function gerarResultado() {
  const jogador = rolarDado() + rolarDado();
  const banca = rolarDado() + rolarDado();
  if (jogador > banca) return 'Jogador';
  if (banca > jogador) return 'Banca';
  return 'Empate';
}

function simularJogos(qtd) {
  const resultados = [];
  for (let i = 0; i < qtd; i++) {
    resultados.push(gerarResultado());
  }
  return resultados;
}

export default function App() {
  const [historico, setHistorico] = useState([]);
  const [sugestao, setSugestao] = useState('');

  useEffect(() => {
    setHistorico(simularJogos(20));
  }, []);

  useEffect(() => {
    const contagem = { Jogador: 0, Banca: 0, Empate: 0 };
    historico.forEach(r => contagem[r]++);
    const maisFrequente = Object.keys(contagem).reduce((a, b) => contagem[a] > contagem[b] ? a : b);
    setSugestao(maisFrequente);
  }, [historico]);

  const calcularPercentuais = () => {
    const total = historico.length;
    const contagem = { Jogador: 0, Banca: 0, Empate: 0 };
    historico.forEach(r => contagem[r]++);
    return {
      Jogador: ((contagem.Jogador / total) * 100).toFixed(1),
      Banca: ((contagem.Banca / total) * 100).toFixed(1),
      Empate: ((contagem.Empate / total) * 100).toFixed(1),
    };
  };

  const percentuais = calcularPercentuais();

  return (
    <ScrollView contentContainerStyle={styles.container}>
      <Text style={styles.title}>Bot de Estatísticas Bac Bo</Text>
      <Text style={styles.subtitle}>Baseado em 20 jogos simulados</Text>

      <View style={styles.section}>
        <Text style={styles.sectionTitle}>Histórico:</Text>
        <View style={styles.grid}>
          {historico.map((r, i) => (
            <Text key={i} style={styles.cell}>{r}</Text>
          ))}
        </View>
      </View>

      <Text style={styles.percent}>Banca: {percentuais.Banca}%</Text>
      <Text style={styles.percent}>Jogador: {percentuais.Jogador}%</Text>
      <Text style={styles.percent}>Empate: {percentuais.Empate}%</Text>

      <View style={styles.suggestionBox}>
        <Text style={styles.suggestionTitle}>Sugestão de Aposta:</Text>
        <Text style={styles.suggestionText}>{sugestao}</Text>
      </View>

      <Button title="Simular Novos Jogos" onPress={() => setHistorico(simularJogos(20))} />
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  container: {
    padding: 20,
    alignItems: 'center',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 10,
  },
  subtitle: {
    fontSize: 16,
    marginBottom: 20,
  },
  section: {
    marginBottom: 20,
    alignItems: 'center',
  },
  sectionTitle: {
    fontSize: 18,
    fontWeight: '600',
    marginBottom: 10,
  },
  grid: {
    flexDirection: 'row',
    flexWrap: 'wrap',
    gap: 4,
    justifyContent: 'center',
  },
  cell: {
    padding: 6,
    margin: 2,
    backgroundColor: '#eee',
    borderRadius: 4,
    width: 60,
    textAlign: 'center',
  },
  percent: {
    fontSize: 16,
  },
  suggestionBox: {
    marginTop: 20,
    padding: 10,
    backgroundColor: '#d0ebff',
    borderRadius: 8,
    alignItems: 'center',
  },
  suggestionTitle: {
    fontSize: 18,
    fontWeight: '600',
  },
  suggestionText: {
    fontSize: 24,
    color: '#1c7ed6',
    fontWeight: 'bold',
  },
});

