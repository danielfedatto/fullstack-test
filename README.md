# fullstack-test

##Arquitetura limpa e DDD
Explique rapidamente (em até 3 frases) a diferença entre “camada de domínio” e “camada de aplicação”
dentro da arquitetura limpa. Dê um exemplo prático (em pseudocódigo ou TypeScript) de uma entidade
de domínio “Task”.

Resposta:

A Clean Architecture foca na separação do código em camadas para manter o sistema organizado, facilitando a manutenção, a escalabilidade e a testabilidade. Ela não se preocupa especificamente com o domínio do negócio, mas sim com a estrutura geral do software. A camada de domínio é uma abordagem que coloca o domínio do negócio no centro do desenvolvimento e procura envolver os especialistas de domínio no processo de design. Já a camada de aplicacao define as tarefas que o software deve realizar e direciona os objetos de domínio expressivos para a resolução de problemas. As tarefas pelas quais esta camada é responsável são significativas para o negócio ou necessárias para a interação com as camadas de aplicação de outros sistemas.

##Padrões de Projeto
Você precisa acessar dados de tarefas a partir de diferentes bancos (PostgreSQL e MongoDB). Qual
padrão de projeto usaria para isolar essa lógica e manter o código flexível? Mostre um pequeno
exemplo de código TypeScript.

´
interface Tarefas {
    id: string;
    title: string;
    completed: boolean;
}
interface RepositorioTarefas {
    findById(id: string): Promise<Task>;
    save(task: Task): Promise<void>;
}
class PostgressRepositorioTarefas implements RepositorioTarefas {
    async findById(id: string): Promise<Tarefas> {
        
        return { id, title: "Tarefas pelo Postgres", completed: false };
    }

    async save(task: Task): Promise<void> {
        ...
    }
}
class MongoRepositorioTarefas implements RepositorioTarefas {
    async findById(id: string): Promise<Task> {
        return { id, title: "Task from MongoDB", completed: false };
    }

    async save(task: Task): Promise<void> {
        ...
    }
}

class RepositorioTarefasFac {
    static criarRepositorio(type: "postgres" | "mongodb"): RepositorioTarefas {
        switch (type) {
            case "postgres":
                return new PostgressRepositorioTarefas();
            case "mongodb":
                return new MongoRepositorioTarefas();
            default:
                throw new Error("Database não suportada");
        }
    }
}

async function main() {
    const postgresRepo = RepositorioTarefasFac.criarRepositorio("postgres");
    const mongoRepo = RepositorioTarefasFac.criarRepositorio("mongodb");

    const task1 = await postgresRepo.findById("123");
    const task2 = await mongoRepo.findById("456");
}
´

##Node.js + Express
Escreva uma rota simples em Express que retorne todas as tarefas de um usuário autenticado. O ID do
usuário virá no req.user.id. Retorne um JSON no formato: { "tasks": [ { "id": 1, "title": "Fazer deploy" } ] }
Resposta:

´
import express from 'express';
import { Router } from 'express';

const router = Router();

const authMiddleware = (req: any, res: any, next: any) => {
  if (!req.user) {
    return res.status(401).json({ error: 'Não Autorizado' });
  }
  next();
};


router.get('/tasks', authMiddleware, async (req: any, res: any) => {
  try {
    
    const tasks = [
      { id: 1, title: "Fazer deploy" },
      { id: 2, title: "Implementar autenticação" }
    ];

    return res.json({ tasks });
  } catch (error) {
    return res.status(500).json({ error: 'Erro interno' });
  }
});

export default router;
´

##Next.js + TailwindCSS
Explique (em uma frase) a diferença entre Server Components e Client Components no Next.js 13+.
Mostre um exemplo rápido de um botão estilizado com TailwindCSS que dispara uma função
handleAddTask().
Resposta:

Os Server Components são componentes renderizados no servidor e não incluem interatividade do cliente, enquanto Client Components (marcados com 'use client') são interativos e executam JavaScript no navegador.

´
'use client'

export default function AddTaskButton() {
  const handleAddTask = () => {
    ...
    console.log('Adicionando nova tarefa...');
  };

  return (
    <button 
      onClick={handleAddTask}
      className="bg-blue-500 hover:bg-blue-700 text-white"
    >
      Adicionar Tarefa
    </button>
  );
}
´

##Git e Boas Práticas
Você está desenvolvendo uma nova funcionalidade chamada “criar tarefa”. Escreva três nomes de
commits claros e padronizados que você usaria.
Resposta:

feat: adiciona estrutura base de criação de tarefas
feat: implementa validação e persistência de novas tarefas
test: adiciona testes unitários para criação de tarefas

##TypeScript – Tipagem
Crie uma interface TypeScript chamada Task com os campos id, title, completed, createdAt. Declare uma
variável tasks que seja um array dessa interface e adicione duas tarefas de exemplo.
Resposta:

´
interface Task {
    id: number;
    title: string;
    completed: boolean;
    createdAt: Date;
}

const tasks: Task[] = [
    {
        id: 1,
        title: "Title",
        completed: false,
        createdAt: new Date()
    },
    {
        id: 2,
        title: "Title2",
        completed: true,
        createdAt: new Date("2025-10-27")
    }
];
´

##Next.js – API Routes
Crie uma API Route em Next.js chamada /api/tasks que retorne uma lista de tarefas simulada em JSON.
Resposta:

´
import { NextResponse } from 'next/server';

type Task = {
  id: number;
  title: string;
  completed: boolean;
  createdAt: string;
};

const tasks: Task[] = [
  { id: 1, title: 'Title', completed: false, createdAt: new Date().toISOString() },
  { id: 2, title: 'Title2', completed: true, createdAt: '2025-10-25T00:00:00.000Z' },
];

export async function GET() {
  return NextResponse.json({ tasks });
}
´

##Node.js – Manipulação de Dados
Escreva uma função TypeScript que recebe um array de tarefas (Task[]) e retorna somente as tarefas
que não foram concluídas (completed === false).
Resposta:

´
const getIncompleteTasks = (tasks: Task[]): Task[] => tasks.filter(task => !task.completed);
´

##TailwindCSS – Estilização
Mostre como criar uma lista de tarefas usando TailwindCSS, onde cada item da lista: - Tem borda
arredondada (rounded) - Tem sombra leve (shadow-sm) - Muda a cor de fundo ao passar o mouse
(hover:bg-gray-100)
Resposta:

´
export default function TaskList({ tasks }) {
  return (
    <ul className="space-y-2">
      {tasks.map((task) => (
        <li 
          key={task.id}
          className="p-4 rounded shadow-sm hover:bg-gray-100 transition-colors duration-200 border border-gray-200"
        >
          <div className="flex items-center justify-between">
            <span className="text-gray-800">{task.title}</span>
            <span className={`text-sm ${task.completed ? 'text-green-500' : 'text-gray-500'}`}>
              {task.completed ? 'Concluída' : 'Pendente'}
            </span>
          </div>
        </li>
      ))}
    </ul>
  );
}
´

Boas práticas de código
Você está trabalhando em uma função que cria uma nova tarefa, mas percebe que o código está
ficando muito longo e repetitivo. Cite duas práticas ou técnicas para melhorar a organização e explique
uma linha de raciocínio de arquitetura limpa ou DDD que aplicaria.
Resposta:

Nesse caso usaria o Princípio da Responsabilidade Única (SRP) para separar a lógica de criação de tarefa em funções menores e especializadas e o Value Objects e DTOs que consiste em usar objetos de valor para encapsular regras de validação

Eu aplicaria o SRP da seguinte forma:

function validateTaskData(taskData: TaskDTO): ValidationResult {
    // Lógica de validação isolada
}

function createTaskEntity(taskData: TaskDTO): Task {
    return new Task({
        title: taskData.title,
        createdAt: new Date(),
        completed: false
    });
}
