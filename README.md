# 🚀 Leque de Vagas — API REST (Spring Boot)

API RESTful desenvolvida com Spring Boot, aplicando arquitetura em camadas e repositórios simulados em memória (sem dependência de banco de dados relacional e sem valores nulos).

---

## 🔀 Roteiro Git para a Equipe (Início Imediato)

> **Atenção:** As branches de cada frente **já foram criadas no repositório remoto**. Ninguém deve criar branches novas nem commitar diretamente na branch `main`.

Cada pessoa da equipe deve clonar o projeto, sincronizar as branches remotas e mudar imediatamente para a branch designada para a sua frente de desenvolvimento:

### 1. Clonar o projeto e sincronizar
```bash
git clone <URL_DO_REPOSITORIO>
cd <NOME_DA_PASTA_DO_PROJETO>
git fetch --all
```

### 2. Acessar a sua branch designada
* **Pessoa 1 (Frente 1 - Vagas):**
  ```bash
  git checkout feature/frente-1-vaga
  git pull origin feature/frente-1-vaga
  ```
* **Pessoa 2 (Frente 2 - Empresas):**
  ```bash
  git checkout feature/frente-2-empresa
  git pull origin feature/frente-2-empresa
  ```
* **Pessoa 3 (Frente 3 - Candidaturas):**
  ```bash
  git checkout feature/frente-3-candidatura
  git pull origin feature/frente-3-candidatura
  ```
* **Pessoa 4 (Frente 4 - Busca & Estatísticas):**
  ```bash
  git checkout feature/frente-4-busca-metricas
  git pull origin feature/frente-4-busca-metricas
  ```

### 3. Fluxo de trabalho diário
1. Certifique-se de estar na sua branch: `git branch`
2. Escreva o código respectivo da sua frente.
3. Adicione e envie suas alterações:
   ```bash
   git add .
   git commit -m "feat: implementacao da frente X"
   git push origin <SUA_BRANCH>
   ```
4. Ao concluir, abra um **Pull Request (PR)** da sua branch apontando para a branch `main`.

---

## 📌 Sumário
- [Arquitetura e Conceitos](#-arquitetura-e-conceitos)
- [Estrutura de Pastas do Projeto](#-estrutura-de-pastas-do-projeto)
- [Divisão das Frentes de Trabalho](#-divisão-das-frentes-de-trabalho)
- [Base Comum (Tratamento de Erros)](#-base-comum-tratamento-de-erros)
- [Frente 1: Gestão de Vagas](#-frente-1-gestão-de-vagas)
- [Frente 2: Empresas](#-frente-2-empresas)
- [Frente 3: Candidaturas](#-frente-3-candidaturas)
- [Frente 4: Busca & Estatísticas](#-frente-4-busca--estatísticas)

---

## 🏛 Arquitetura e Conceitos

- **Controller (`@RestController`):** Porta de entrada HTTP, mapeamento de rotas e retorno de códigos de status HTTP (200, 201, 204, 400, 404).
- **Service (`@Service`):** Regras de negócio, validações de domínio e conversão entre modelos e DTOs.
- **Repository (`@Repository`):** Persistência simulada em memória através de coleções Java (`Map`, `List`) e identificadores atômicos.
- **DTOs:** Records Java imutáveis para transferência segura de dados de entrada e saída.

---

## 📂 Estrutura de Pastas do Projeto

```text
src/
└── main/
    ├── java/
    │   └── lequedevagas/
    │       └── demo/
    │           ├── controller/
    │           │   ├── BuscaEstatisticasController.java
    │           │   ├── CandidaturaController.java
    │           │   ├── EmpresaController.java
    │           │   ├── GlobalExceptionHandler.java
    │           │   └── VagaController.java
    │           ├── dto/
    │           │   ├── ErroRespostaDTO.java
    │           │   ├── busca/
    │           │   │   └── EstatisticasRespostaDTO.java
    │           │   ├── candidatura/
    │           │   │   ├── CandidaturaRequisicaoDTO.java
    │           │   │   └── CandidaturaRespostaDTO.java
    │           │   ├── empresa/
    │           │   │   ├── EmpresaRequisicaoDTO.java
    │           │   │   └── EmpresaRespostaDTO.java
    │           │   └── vaga/
    │           │       ├── VagaRequisicaoDTO.java
    │           │       └── VagaRespostaDTO.java
    │           ├── exception/
    │           │   └── RecursoNaoEncontradoException.java
    │           ├── model/
    │           │   ├── Candidatura.java
    │           │   ├── Empresa.java
    │           │   ├── Pessoa.java
    │           │   └── Vaga.java
    │           ├── repository/
    │           │   ├── CandidaturaRepository.java
    │           │   ├── EmpresaRepository.java
    │           │   └── VagaRepository.java
    │           ├── service/
    │           │   ├── BuscaEstatisticasService.java
    │           │   ├── CandidaturaService.java
    │           │   ├── EmpresaService.java
    │           │   └── VagaService.java
    │           └── DemoApplication.java
    └── resources/
        └── application.properties
```

---

## 👥 Divisão das Frentes de Trabalho

| Frente | Responsável | Módulo | Branch Existente | Endpoints |
| :--- | :--- | :--- | :--- | :--- |
| **Frente 1** | Pessoa 1 | Gestão de Vagas | `feature/frente-1-vaga` | `GET /vagas`, `GET /vagas/{id}`, `POST /vagas`, `PUT /vagas/{id}`, `DELETE /vagas/{id}` |
| **Frente 2** | Pessoa 2 | Empresas Parceiras | `feature/frente-2-empresa` | `GET /empresas`, `GET /empresas/{id}`, `POST /empresas` |
| **Frente 3** | Pessoa 3 | Candidaturas | `feature/frente-3-candidatura` | `POST /pessoas`, `GET /candidaturas`, `POST /candidaturas` |
| **Frente 4** | Pessoa 4 | Busca & Estatísticas | `feature/frente-4-busca-metricas` | `GET /estatisticas`, `GET /vagas/busca` |

---

## ⚙️ Base Comum (Tratamento de Erros)

### `dto/ErroRespostaDTO.java`
```java
package lequedevagas.demo.dto;

import java.time.LocalDateTime;
import java.util.List;

public record ErroRespostaDTO(
    LocalDateTime timestamp,
    int status,
    String erro,
    List<String> detalhes
) {}
```

### `exception/RecursoNaoEncontradoException.java`
```java
package lequedevagas.demo.exception;

public class RecursoNaoEncontradoException extends RuntimeException {
    public RecursoNaoEncontradoException(String mensagem) {
        super(mensagem);
    }
}
```

### `controller/GlobalExceptionHandler.java`
```java
package lequedevagas.demo.controller;

import lequedevagas.demo.dto.ErroRespostaDTO;
import lequedevagas.demo.exception.RecursoNaoEncontradoException;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import java.time.LocalDateTime;
import java.util.List;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(RecursoNaoEncontradoException.class)
    public ResponseEntity<ErroRespostaDTO> tratarNaoEncontrado(RecursoNaoEncontradoException ex) {
        ErroRespostaDTO erro = new ErroRespostaDTO(
            LocalDateTime.now(),
            HttpStatus.NOT_FOUND.value(),
            "Recurso não encontrado",
            List.of(ex.getMessage())
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(erro);
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErroRespostaDTO> tratarValidacao(MethodArgumentNotValidException ex) {
        List<String> erros = ex.getBindingResult()
            .getFieldErrors()
            .stream()
            .map(f -> f.getField() + ": " + f.getDefaultMessage())
            .toList();

        ErroRespostaDTO erro = new ErroRespostaDTO(
            LocalDateTime.now(),
            HttpStatus.BAD_REQUEST.value(),
            "Erro de validação",
            erros
        );
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(erro);
    }
}
```

---

## 💼 Frente 1: Gestão de Vagas

### `model/Vaga.java`
```java
package lequedevagas.demo.model;

import java.time.LocalDate;

public class Vaga {
    private Long id;
    private String titulo;
    private String descricao;
    private String area;
    private String senioridade;
    private String local;
    private Boolean aceitaIniciante;
    private Long empresaId;
    private LocalDate publicadaEm;

    public Vaga(String titulo, String descricao, String area, String senioridade, String local, Boolean aceitaIniciante, Long empresaId) {
        this.titulo = titulo;
        this.descricao = descricao;
        this.area = area;
        this.senioridade = senioridade;
        this.local = local;
        this.aceitaIniciante = aceitaIniciante;
        this.empresaId = empresaId;
    }

    public Vaga(Long id, String titulo, String descricao, String area, String senioridade, String local, Boolean aceitaIniciante, Long empresaId, LocalDate publicadaEm) {
        this.id = id;
        this.titulo = titulo;
        this.descricao = descricao;
        this.area = area;
        this.senioridade = senioridade;
        this.local = local;
        this.aceitaIniciante = aceitaIniciante;
        this.empresaId = empresaId;
        this.publicadaEm = publicadaEm;
    }

    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getTitulo() { return titulo; }
    public void setTitulo(String titulo) { this.titulo = titulo; }
    public String getDescricao() { return descricao; }
    public void setDescricao(String descricao) { this.descricao = descricao; }
    public String getArea() { return area; }
    public void setArea(String area) { this.area = area; }
    public String getSenioridade() { return senioridade; }
    public void setSenioridade(String senioridade) { this.senioridade = senioridade; }
    public String getLocal() { return local; }
    public void setLocal(String local) { this.local = local; }
    public Boolean getAceitaIniciante() { return aceitaIniciante; }
    public void setAceitaIniciante(Boolean aceitaIniciante) { this.aceitaIniciante = aceitaIniciante; }
    public Long getEmpresaId() { return empresaId; }
    public void setEmpresaId(Long empresaId) { this.empresaId = empresaId; }
    public LocalDate getPublicadaEm() { return publicadaEm; }
    public void setPublicadaEm(LocalDate publicadaEm) { this.publicadaEm = publicadaEm; }
}
```

### `dto/vaga/VagaRequisicaoDTO.java`
```java
package lequedevagas.demo.dto.vaga;

import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;

public record VagaRequisicaoDTO(
    @NotBlank(message = "Título é obrigatório") String titulo,
    @NotBlank(message = "Descrição é obrigatória") String descricao,
    @NotBlank(message = "Área é obrigatória") String area,
    @NotBlank(message = "Senioridade é obrigatória") String senioridade,
    @NotBlank(message = "Local é obrigatório") String local,
    @NotNull(message = "Informe se aceita iniciante") Boolean aceitaIniciante,
    @NotNull(message = "Empresa ID é obrigatório") Long empresaId
) {}
```

### `dto/vaga/VagaRespostaDTO.java`
```java
package lequedevagas.demo.dto.vaga;

import lequedevagas.demo.model.Vaga;
import java.time.LocalDate;

public record VagaRespostaDTO(
    Long id,
    String titulo,
    String descricao,
    String area,
    String senioridade,
    String local,
    Boolean aceitaIniciante,
    Long empresaId,
    LocalDate publicadaEm
) {
    public static VagaRespostaDTO fromModel(Vaga vaga) {
        return new VagaRespostaDTO(
            vaga.getId(),
            vaga.getTitulo(),
            vaga.getDescricao(),
            vaga.getArea(),
            vaga.getSenioridade(),
            vaga.getLocal(),
            vaga.getAceitaIniciante(),
            vaga.getEmpresaId(),
            vaga.getPublicadaEm()
        );
    }
}
```

### `repository/VagaRepository.java`
```java
package lequedevagas.demo.repository;

import lequedevagas.demo.model.Vaga;
import org.springframework.stereotype.Repository;

import java.time.LocalDate;
import java.util.*;
import java.util.concurrent.atomic.AtomicLong;

@Repository
public class VagaRepository {
    private final Map<Long, Vaga> tabelaVagas = new HashMap<>();
    private final AtomicLong geradorId = new AtomicLong(1);

    public VagaRepository() {
        long id1 = geradorId.getAndIncrement();
        tabelaVagas.put(id1, new Vaga(id1, "Desenvolvedor Backend Júnior", "Vaga para atuar com Java e APIs REST", "Back-end", "Júnior", "Remoto", true, 1L, LocalDate.now()));

        long id2 = geradorId.getAndIncrement();
        tabelaVagas.put(id2, new Vaga(id2, "Estagiário Frontend", "Auxiliar no Next.js do produto principal", "Front-end", "Estágio", "São Paulo, SP", true, 1L, LocalDate.now()));
    }

    public List<Vaga> listarTodas() {
        return new ArrayList<>(tabelaVagas.values());
    }

    public Optional<Vaga> buscarPorId(Long id) {
        return Optional.ofNullable(tabelaVagas.get(id));
    }

    public Vaga salvar(Vaga vaga) {
        if (vaga.getId() == null) {
            vaga.setId(geradorId.getAndIncrement());
            vaga.setPublicadaEm(LocalDate.now());
        }
        tabelaVagas.put(vaga.getId(), vaga);
        return vaga;
    }

    public void deletar(Long id) {
        tabelaVagas.remove(id);
    }
}
```

### `service/VagaService.java`
```java
package lequedevagas.demo.service;

import lequedevagas.demo.dto.vaga.VagaRequisicaoDTO;
import lequedevagas.demo.dto.vaga.VagaRespostaDTO;
import lequedevagas.demo.exception.RecursoNaoEncontradoException;
import lequedevagas.demo.model.Vaga;
import lequedevagas.demo.repository.VagaRepository;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class VagaService {

    private final VagaRepository vagaRepository;

    public VagaService(VagaRepository vagaRepository) {
        this.vagaRepository = vagaRepository;
    }

    public List<VagaRespostaDTO> listarTodas() {
        return vagaRepository.listarTodas().stream()
            .map(VagaRespostaDTO::fromModel)
            .toList();
    }

    public VagaRespostaDTO buscarPorId(Long id) {
        Vaga vaga = vagaRepository.buscarPorId(id)
            .orElseThrow(() -> new RecursoNaoEncontradoException("Vaga com ID " + id + " não encontrada"));
        return VagaRespostaDTO.fromModel(vaga);
    }

    public VagaRespostaDTO criar(VagaRequisicaoDTO dto) {
        Vaga vaga = new Vaga(
            dto.titulo(),
            dto.descricao(),
            dto.area(),
            dto.senioridade(),
            dto.local(),
            dto.aceitaIniciante(),
            dto.empresaId()
        );
        return VagaRespostaDTO.fromModel(vagaRepository.salvar(vaga));
    }

    public VagaRespostaDTO atualizar(Long id, VagaRequisicaoDTO dto) {
        Vaga existente = vagaRepository.buscarPorId(id)
            .orElseThrow(() -> new RecursoNaoEncontradoException("Vaga com ID " + id + " não encontrada"));

        existente.setTitulo(dto.titulo());
        existente.setDescricao(dto.descricao());
        existente.setArea(dto.area());
        existente.setSenioridade(dto.senioridade());
        existente.setLocal(dto.local());
        existente.setAceitaIniciante(dto.aceitaIniciante());
        existente.setEmpresaId(dto.empresaId());

        return VagaRespostaDTO.fromModel(vagaRepository.salvar(existente));
    }

    public void deletar(Long id) {
        vagaRepository.buscarPorId(id)
            .orElseThrow(() -> new RecursoNaoEncontradoException("Vaga com ID " + id + " não encontrada"));
        vagaRepository.deletar(id);
    }
}
```

### `controller/VagaController.java`
```java
package lequedevagas.demo.controller;

import lequedevagas.demo.dto.vaga.VagaRequisicaoDTO;
import lequedevagas.demo.dto.vaga.VagaRespostaDTO;
import lequedevagas.demo.service.VagaService;
import jakarta.validation.Valid;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/vagas")
public class VagaController {

    private final VagaService vagaService;

    public VagaController(VagaService vagaService) {
        this.vagaService = vagaService;
    }

    @GetMapping
    public ResponseEntity<List<VagaRespostaDTO>> listar() {
        return ResponseEntity.ok(vagaService.listarTodas());
    }

    @GetMapping("/{id}")
    public ResponseEntity<VagaRespostaDTO> buscarPorId(@PathVariable Long id) {
        return ResponseEntity.ok(vagaService.buscarPorId(id));
    }

    @PostMapping
    public ResponseEntity<VagaRespostaDTO> criar(@RequestBody @Valid VagaRequisicaoDTO dto) {
        VagaRespostaDTO criada = vagaService.criar(dto);
        return ResponseEntity.status(HttpStatus.CREATED).body(criada);
    }

    @PutMapping("/{id}")
    public ResponseEntity<VagaRespostaDTO> atualizar(@PathVariable Long id, @RequestBody @Valid VagaRequisicaoDTO dto) {
        return ResponseEntity.ok(vagaService.atualizar(id, dto));
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deletar(@PathVariable Long id) {
        vagaService.deletar(id);
        return ResponseEntity.noContent().build();
    }
}
```

---

## 🏢 Frente 2: Empresas

### `model/Empresa.java`
```java
package lequedevagas.demo.model;

public class Empresa {
    private Long id;
    private String nome;
    private String slug;
    private String site;
    private String descricao;

    public Empresa(String nome, String slug, String site, String descricao) {
        this.nome = nome;
        this.slug = slug;
        this.site = site;
        this.descricao = descricao;
    }

    public Empresa(Long id, String nome, String slug, String site, String descricao) {
        this.id = id;
        this.nome = nome;
        this.slug = slug;
        this.site = site;
        this.descricao = descricao;
    }

    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getNome() { return nome; }
    public void setNome(String nome) { this.nome = nome; }
    public String getSlug() { return slug; }
    public void setSlug(String slug) { this.slug = slug; }
    public String getSite() { return site; }
    public void setSite(String site) { this.site = site; }
    public String getDescricao() { return descricao; }
    public void setDescricao(String descricao) { this.descricao = descricao; }
}
```

### `dto/empresa/EmpresaRequisicaoDTO.java`
```java
package lequedevagas.demo.dto.empresa;

import jakarta.validation.constraints.NotBlank;

public record EmpresaRequisicaoDTO(
    @NotBlank(message = "Nome da empresa é obrigatório") String nome,
    @NotBlank(message = "Slug identificador é obrigatório") String slug,
    @NotBlank(message = "Site institucional é obrigatório") String site,
    @NotBlank(message = "Descrição é obrigatória") String descricao
) {}
```

### `dto/empresa/EmpresaRespostaDTO.java`
```java
package lequedevagas.demo.dto.empresa;

import lequedevagas.demo.model.Empresa;

public record EmpresaRespostaDTO(
    Long id,
    String nome,
    String slug,
    String site,
    String descricao
) {
    public static EmpresaRespostaDTO fromModel(Empresa e) {
        return new EmpresaRespostaDTO(e.getId(), e.getNome(), e.getSlug(), e.getSite(), e.getDescricao());
    }
}
```

### `repository/EmpresaRepository.java`
```java
package lequedevagas.demo.repository;

import lequedevagas.demo.model.Empresa;
import org.springframework.stereotype.Repository;

import java.util.*;
import java.util.concurrent.atomic.AtomicLong;

@Repository
public class EmpresaRepository {
    private final Map<Long, Empresa> tabelaEmpresas = new HashMap<>();
    private final AtomicLong geradorId = new AtomicLong(1);

    public EmpresaRepository() {
        long id1 = geradorId.getAndIncrement();
        tabelaEmpresas.put(id1, new Empresa(id1, "TechInclusiva", "tech-inclusiva", "portal-techinclusiva", "Empresa focada em formação e recrutamento de juniores."));
    }

    public List<Empresa> listarTodas() {
        return new ArrayList<>(tabelaEmpresas.values());
    }

    public Optional<Empresa> buscarPorId(Long id) {
        return Optional.ofNullable(tabelaEmpresas.get(id));
    }

    public Empresa salvar(Empresa empresa) {
        if (empresa.getId() == null) {
            empresa.setId(geradorId.getAndIncrement());
        }
        tabelaEmpresas.put(empresa.getId(), empresa);
        return empresa;
    }
}
```

### `service/EmpresaService.java`
```java
package lequedevagas.demo.service;

import lequedevagas.demo.dto.empresa.EmpresaRequisicaoDTO;
import lequedevagas.demo.dto.empresa.EmpresaRespostaDTO;
import lequedevagas.demo.exception.RecursoNaoEncontradoException;
import lequedevagas.demo.model.Empresa;
import lequedevagas.demo.repository.EmpresaRepository;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class EmpresaService {

    private final EmpresaRepository empresaRepository;

    public EmpresaService(EmpresaRepository empresaRepository) {
        this.empresaRepository = empresaRepository;
    }

    public List<EmpresaRespostaDTO> listarTodas() {
        return empresaRepository.listarTodas().stream()
            .map(EmpresaRespostaDTO::fromModel)
            .toList();
    }

    public EmpresaRespostaDTO buscarPorId(Long id) {
        Empresa empresa = empresaRepository.buscarPorId(id)
            .orElseThrow(() -> new RecursoNaoEncontradoException("Empresa com ID " + id + " não encontrada"));
        return EmpresaRespostaDTO.fromModel(empresa);
    }

    public EmpresaRespostaDTO criar(EmpresaRequisicaoDTO dto) {
        Empresa empresa = new Empresa(dto.nome(), dto.slug(), dto.site(), dto.descricao());
        return EmpresaRespostaDTO.fromModel(empresaRepository.salvar(empresa));
    }
}
```

### `controller/EmpresaController.java`
```java
package lequedevagas.demo.controller;

import lequedevagas.demo.dto.empresa.EmpresaRequisicaoDTO;
import lequedevagas.demo.dto.empresa.EmpresaRespostaDTO;
import lequedevagas.demo.service.EmpresaService;
import jakarta.validation.Valid;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/empresas")
public class EmpresaController {

    private final EmpresaService empresaService;

    public EmpresaController(EmpresaService empresaService) {
        this.empresaService = empresaService;
    }

    @GetMapping
    public ResponseEntity<List<EmpresaRespostaDTO>> listar() {
        return ResponseEntity.ok(empresaService.listarTodas());
    }

    @GetMapping("/{id}")
    public ResponseEntity<EmpresaRespostaDTO> buscarPorId(@PathVariable Long id) {
        return ResponseEntity.ok(empresaService.buscarPorId(id));
    }

    @PostMapping
    public ResponseEntity<EmpresaRespostaDTO> criar(@RequestBody @Valid EmpresaRequisicaoDTO dto) {
        return ResponseEntity.status(HttpStatus.CREATED).body(empresaService.criar(dto));
    }
}
```

---

## 👥 Frente 3: Candidaturas

### `model/Candidatura.java`
```java
package lequedevagas.demo.model;

import java.time.LocalDateTime;

public class Candidatura {
    private Long id;
    private Long pessoaId;
    private Long vagaId;
    private String situacao;
    private LocalDateTime enviadaEm;

    public Candidatura(Long pessoaId, Long vagaId) {
        this.pessoaId = pessoaId;
        this.vagaId = vagaId;
    }

    public Candidatura(Long id, Long pessoaId, Long vagaId, String situacao, LocalDateTime enviadaEm) {
        this.id = id;
        this.pessoaId = pessoaId;
        this.vagaId = vagaId;
        this.situacao = situacao;
        this.enviadaEm = enviadaEm;
    }

    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public Long getPessoaId() { return pessoaId; }
    public void setPessoaId(Long pessoaId) { this.pessoaId = pessoaId; }
    public Long getVagaId() { return vagaId; }
    public void setVagaId(Long vagaId) { this.vagaId = vagaId; }
    public String getSituacao() { return situacao; }
    public void setSituacao(String situacao) { this.situacao = situacao; }
    public LocalDateTime getEnviadaEm() { return enviadaEm; }
    public void setEnviadaEm(LocalDateTime enviadaEm) { this.enviadaEm = enviadaEm; }
}
```

### `dto/candidatura/CandidaturaRequisicaoDTO.java`
```java
package lequedevagas.demo.dto.candidatura;

import jakarta.validation.constraints.NotNull;

public record CandidaturaRequisicaoDTO(
    @NotNull(message = "Pessoa ID é obrigatório") Long pessoaId,
    @NotNull(message = "Vaga ID é obrigatório") Long vagaId
) {}
```

### `dto/candidatura/CandidaturaRespostaDTO.java`
```java
package lequedevagas.demo.dto.candidatura;

import lequedevagas.demo.model.Candidatura;
import java.time.LocalDateTime;

public record CandidaturaRespostaDTO(
    Long id,
    Long pessoaId,
    Long vagaId,
    String situacao,
    LocalDateTime enviadaEm
) {
    public static CandidaturaRespostaDTO fromModel(Candidatura c) {
        return new CandidaturaRespostaDTO(c.getId(), c.getPessoaId(), c.getVagaId(), c.getSituacao(), c.getEnviadaEm());
    }
}
```

### `repository/CandidaturaRepository.java`
```java
package lequedevagas.demo.repository;

import lequedevagas.demo.model.Candidatura;
import org.springframework.stereotype.Repository;

import java.time.LocalDateTime;
import java.util.*;
import java.util.concurrent.atomic.AtomicLong;

@Repository
public class CandidaturaRepository {
    private final Map<Long, Candidatura> tabelaCandidaturas = new HashMap<>();
    private final AtomicLong geradorId = new AtomicLong(1);

    public Candidatura salvar(Candidatura c) {
        if (c.getId() == null) {
            c.setId(geradorId.getAndIncrement());
            c.setEnviadaEm(LocalDateTime.now());
            c.setSituacao("RECEBIDA");
        }
        tabelaCandidaturas.put(c.getId(), c);
        return c;
    }

    public List<Candidatura> listarTodas() {
        return new ArrayList<>(tabelaCandidaturas.values());
    }
}
```

### `service/CandidaturaService.java`
```java
package lequedevagas.demo.service;

import lequedevagas.demo.dto.candidatura.CandidaturaRequisicaoDTO;
import lequedevagas.demo.dto.candidatura.CandidaturaRespostaDTO;
import lequedevagas.demo.exception.RecursoNaoEncontradoException;
import lequedevagas.demo.model.Candidatura;
import lequedevagas.demo.repository.CandidaturaRepository;
import lequedevagas.demo.repository.VagaRepository;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class CandidaturaService {

    private final CandidaturaRepository candidaturaRepository;
    private final VagaRepository vagaRepository;

    public CandidaturaService(CandidaturaRepository candidaturaRepository, VagaRepository vagaRepository) {
        this.candidaturaRepository = candidaturaRepository;
        this.vagaRepository = vagaRepository;
    }

    public CandidaturaRespostaDTO candidatar(CandidaturaRequisicaoDTO dto) {
        vagaRepository.buscarPorId(dto.vagaId())
            .orElseThrow(() -> new RecursoNaoEncontradoException("Não é possível candidatar: Vaga ID " + dto.vagaId() + " não existe"));

        Candidatura candidatura = new Candidatura(dto.pessoaId(), dto.vagaId());
        return CandidaturaRespostaDTO.fromModel(candidaturaRepository.salvar(candidatura));
    }

    public List<CandidaturaRespostaDTO> listarTodas() {
        return candidaturaRepository.listarTodas().stream()
            .map(CandidaturaRespostaDTO::fromModel)
            .toList();
    }
}
```

### `controller/CandidaturaController.java`
```java
package lequedevagas.demo.controller;

import lequedevagas.demo.dto.candidatura.CandidaturaRequisicaoDTO;
import lequedevagas.demo.dto.candidatura.CandidaturaRespostaDTO;
import lequedevagas.demo.service.CandidaturaService;
import jakarta.validation.Valid;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/candidaturas")
public class CandidaturaController {

    private final CandidaturaService candidaturaService;

    public CandidaturaController(CandidaturaService candidaturaService) {
        this.candidaturaService = candidaturaService;
    }

    @PostMapping
    public ResponseEntity<CandidaturaRespostaDTO> criar(@RequestBody @Valid CandidaturaRequisicaoDTO dto) {
        return ResponseEntity.status(HttpStatus.CREATED).body(candidaturaService.candidatar(dto));
    }

    @GetMapping
    public ResponseEntity<List<CandidaturaRespostaDTO>> listar() {
        return ResponseEntity.ok(candidaturaService.listarTodas());
    }
}
```

---

## 🔍 Frente 4: Busca & Estatísticas

### `dto/busca/EstatisticasRespostaDTO.java`
```java
package lequedevagas.demo.dto.busca;

public record EstatisticasRespostaDTO(
    int totalVagas,
    int totalEmpresas,
    int totalCandidaturas,
    long vagasParaIniciantes
) {}
```

### `service/BuscaEstatisticasService.java`
```java
package lequedevagas.demo.service;

import lequedevagas.demo.dto.busca.EstatisticasRespostaDTO;
import lequedevagas.demo.dto.vaga.VagaRespostaDTO;
import lequedevagas.demo.repository.CandidaturaRepository;
import lequedevagas.demo.repository.EmpresaRepository;
import lequedevagas.demo.repository.VagaRepository;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class BuscaEstatisticasService {

    private final VagaRepository vagaRepository;
    private final EmpresaRepository empresaRepository;
    private final CandidaturaRepository candidaturaRepository;

    public BuscaEstatisticasService(
        VagaRepository vagaRepository,
        EmpresaRepository empresaRepository,
        CandidaturaRepository candidaturaRepository
    ) {
        this.vagaRepository = vagaRepository;
        this.empresaRepository = empresaRepository;
        this.candidaturaRepository = candidaturaRepository;
    }

    public EstatisticasRespostaDTO calcularEstatisticas() {
        var vagas = vagaRepository.listarTodas();
        int totalEmpresas = empresaRepository.listarTodas().size();
        int totalCandidaturas = candidaturaRepository.listarTodas().size();
        long aceitaIniciante = vagas.stream().filter(v -> Boolean.TRUE.equals(v.getAceitaIniciante())).count();

        return new EstatisticasRespostaDTO(
            vagas.size(),
            totalEmpresas,
            totalCandidaturas,
            aceitaIniciante
        );
    }

    public List<VagaRespostaDTO> buscarVagas(String area, String senioridade) {
        return vagaRepository.listarTodas().stream()
            .filter(v -> area == null || v.getArea().equalsIgnoreCase(area))
            .filter(v -> senioridade == null || v.getSenioridade().equalsIgnoreCase(senioridade))
            .map(VagaRespostaDTO::fromModel)
            .toList();
    }
}
```

### `controller/BuscaEstatisticasController.java`
```java
package lequedevagas.demo.controller;

import lequedevagas.demo.dto.busca.EstatisticasRespostaDTO;
import lequedevagas.demo.dto.vaga.VagaRespostaDTO;
import lequedevagas.demo.service.BuscaEstatisticasService;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class BuscaEstatisticasController {

    private final BuscaEstatisticasService service;

    public BuscaEstatisticasController(BuscaEstatisticasService service) {
        this.service = service;
    }

    @GetMapping("/estatisticas")
    public ResponseEntity<EstatisticasRespostaDTO> getEstatisticas() {
        return ResponseEntity.ok(service.calcularEstatisticas());
    }

    @GetMapping("/vagas/busca")
    public ResponseEntity<List<VagaRespostaDTO>> buscar(
        @RequestParam(required = false) String area,
        @RequestParam(required = false) String senioridade
    ) {
        return ResponseEntity.ok(service.buscarVagas(area, senioridade));
    }
}
```