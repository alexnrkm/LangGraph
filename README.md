# LangGraph Study Repository

Репозиторий для изучения фреймворка LangGraph

## Основной стек

| Библиотека | За что отвечает |
|---|---|
| `langgraph` | Основной фреймворк для графов, состояний, переходов и agent workflow. |
| `langchain` | Базовые абстракции для сообщений, tools, prompt-логики и LLM. |
| `langchain-openai` | Подключение моделей OpenAI. |
| `python-dotenv` | Загрузка переменных окружения из `.env`. |

## Дополнительный стек

| Библиотека | За что отвечает |
|---|---|
| `langgraph-checkpoint-sqlite` | Сохранение состояния графа и checkpoint-ов в SQLite. |
| `aiosqlite` | Асинхронная работа с SQLite. |
| `langchain-community` | Дополнительные loaders и интеграции. |
| `langchain-text-splitters` | Разбиение текста на части для RAG. |
| `beautifulsoup4` | Парсинг HTML и веб-страниц. |

## Последовательность написания Graph кода
1. Создать стейты
    ```python
    class AgentState(TypedDict):
    name: str
    values: List[int]
    operation: str
    result: int
    message: str
    ```

2. Создать ноды через функции, работать в них со стейтами и возвращать стейты
    ```python
    def calculate_node(state: AgentState) -> AgentState:
    """description for ai"""
    if state["operation"] == "*":
        state["result"] = reduce(lambda a, b: a * b, state["values"])
    elif state["operation"] == "+":
        state["result"] = reduce(lambda a, b: a + b, state["values"])
    else:
        print("invalid operation")

    state["message"] = f"Hi {state["name"]}, your answer in {state["result"]}"

    return state
    ```

3. Создать Graph, Создать ноды, назначить EntryPoint, Edges, EndPoint
    ```python
    graph = StateGraph(AgentState)

    graph.add_node("first_node", greeting_node)
    graph.add_node("second_node", age_node)
    graph.add_node("third_node", skills_node)

    graph.set_entry_point("first_node")
    graph.add_edge("first_node", "second_node")
    graph.add_edge("second_node", "third_node")
    graph.set_finish_point("third_node")
    ```

4. Скомпилировать Graph
    ```python
    app = graph.compile()
    ```

5. Запустить Graph передав нужные параметры
    ```python
    result = app.invoke({"name": "Alex", "age": 20, "skills": ["SQL", "Python", "LangGraph"]})
    ```




