# Jutsu-Ranking
Website created with the purpose of facilitating and helping Shinobi RPG players to rank Jutsus from the Naruto series. Made by spider.

import { useState } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Input } from "@/components/ui/input"; import { Button } from "@/components/ui/button"; import { Textarea } from "@/components/ui/textarea";

const categories = [ "Ninjutsu", "Genjutsu", "Taijutsu", "Fūinjutsu", "Kekkei Genkai / Tōta", "Senjutsu", "Dōjutsu", "Hijutsu", "Kinjutsu", "Bukijutsu", "Iryō Ninjutsu", "Shikigami / Kuchiyose", "Outras" ];

const criteria = [ "Destruição", "Velocidade", "Complexidade", "Custo de Chakra", "Versatilidade", "Raridade", "Utilidade Tática", "Dificuldade de Aprendizado", "Alcance", "Impacto Psicológico" ];

export default function JutsuRanker() { const [name, setName] = useState(""); const [category, setCategory] = useState([]); const [rank, setRank] = useState("C"); const [description, setDescription] = useState(""); const [scores, setScores] = useState(Array(criteria.length).fill(5)); const [result, setResult] = useState(null);

const handleSubmit = () => { const total = scores.reduce((acc, val) => acc + parseInt(val), 0); let generalRank = "C"; if (total >= 90) generalRank = "SS / God"; else if (total >= 80) generalRank = "S"; else if (total >= 70) generalRank = "A"; else if (total >= 60) generalRank = "B"; else if (total >= 50) generalRank = "C"; else if (total >= 40) generalRank = "D"; else generalRank = "E";

setResult({ name, category, rank, description, scores, total, generalRank });

};

return ( <div className="p-4 max-w-4xl mx-auto"> <h1 className="text-3xl font-bold mb-4">📜 Shinobi Ryūkō – Rank de Jutsus</h1> <Input placeholder="Nome do Jutsu" value={name} onChange={e => setName(e.target.value)} className="mb-2" /> <Textarea placeholder="Descrição do Jutsu" value={description} onChange={e => setDescription(e.target.value)} className="mb-2" /> <div className="mb-2"> <label className="font-semibold">Categoria:</label> <div className="grid grid-cols-2 gap-2 mt-1"> {categories.map(cat => ( <label key={cat} className="flex items-center space-x-2"> <input type="checkbox" value={cat} onChange={e => { const val = e.target.value; setCategory(prev => e.target.checked ? [...prev, val] : prev.filter(c => c !== val)); }} /> <span>{cat}</span> </label> ))} </div> </div> <div className="mb-4"> <label className="font-semibold">Rank Base (Letra):</label> <select value={rank} onChange={e => setRank(e.target.value)} className="ml-2 border rounded px-2 py-1"> {["E", "D", "C", "B", "A", "S", "SS / God"].map(r => ( <option key={r} value={r}>{r}</option> ))} </select> </div> <div className="grid grid-cols-2 gap-4 mb-4"> {criteria.map((crit, index) => ( <div key={crit} className="flex flex-col"> <label className="font-semibold">{crit}</label> <input type="range" min="1" max="10" value={scores[index]} onChange={e => { const newScores = [...scores]; newScores[index] = e.target.value; setScores(newScores); }} /> <span>Nota: {scores[index]}</span> </div> ))} </div> <Button onClick={handleSubmit}>Calcular Rank Final</Button>

{result && (
    <Card className="mt-6">
      <CardContent className="p-4">
        <h2 className="text-xl font-bold">Resultado: {result.name}</h2>
        <p><strong>Rank Base:</strong> {result.rank}</p>
        <p><strong>Rank Final:</strong> {result.generalRank}</p>
        <p><strong>Categorias:</strong> {result.category.join(", ")}</p>
        <p><strong>Descrição:</strong> {result.description}</p>
        <p><strong>Pontuação Total:</strong> {result.total}</p>
      </CardContent>
    </Card>
  )}
</div>

); }

