
                          Para criar uma aplicação de cadastro de estudantes em Micronaut, siga o passo a passo abaixo:
                                          

Instale o Micronaut CLI seguindo as instruções no site oficial: https://micronaut.io/download.html

Crie um novo projeto Micronaut utilizando o comando mn create-app br.com.minhaplicacao substituindo br.com.minhaplicacao pelo nome do seu pacote.

Crie uma entidade de estudante no pacote domain com os campos necessários, por exemplo:

kotlin
Copy code
package br.com.minhaplicacao.domain;

import io.micronaut.core.annotation.Introspected;

@Introspected
public class Estudante {
    private Long id;
    private String nome;
    private int idade;

    // construtores, getters e setters
}
Crie um serviço de cadastro no pacote service com os métodos para salvar, atualizar, buscar e excluir estudantes:
java
Copy code
package br.com.minhaplicacao.service;

import br.com.minhaplicacao.domain.Estudante;

public interface EstudanteService {
    Estudante salvar(Estudante estudante);
    Estudante atualizar(Long id, Estudante estudante);
    Estudante buscarPorId(Long id);
    void excluir(Long id);
}
Implemente o serviço de cadastro de estudantes na classe EstudanteServiceImpl:
java
Copy code
package br.com.minhaplicacao.service;

import br.com.minhaplicacao.domain.Estudante;

import javax.inject.Singleton;
import java.util.HashMap;
import java.util.Map;

@Singleton
public class EstudanteServiceImpl implements EstudanteService {
    private Map<Long, Estudante> estudantes = new HashMap<>();
    private Long sequencia = 0L;

    @Override
    public Estudante salvar(Estudante estudante) {
        estudante.setId(sequencia++);
        estudantes.put(estudante.getId(), estudante);
        return estudante;
    }

    @Override
    public Estudante atualizar(Long id, Estudante estudante) {
        estudantes.put(id, estudante);
        return estudante;
    }

    @Override
    public Estudante buscarPorId(Long id) {
        return estudantes.get(id);
    }

    @Override
    public void excluir(Long id) {
        estudantes.remove(id);
    }
}
Crie um controlador REST no pacote controller com os endpoints para lidar com as operações de cadastro de estudantes:
kotlin
Copy code
package br.com.minhaplicacao.controller;

import br.com.minhaplicacao.domain.Estudante;
import br.com.minhaplicacao.service.EstudanteService;
import io.micronaut.http.annotation.*;

@Controller("/estudantes")
public class EstudanteController {
    private final EstudanteService estudanteService;

    public EstudanteController(EstudanteService estudanteService) {
        this.estudanteService = estudanteService;
    }

    @Post
    public Estudante cadastrarEstudante(@Body Estudante estudante) {
        return estudanteService.salvar(estudante);
    }

    @Get("/{id}")
    public Estudante buscarEstudante(Long id) {
        return estudanteService.buscarPorId(id);
    }

    @Put("/{id}")
    public Estudante atualizarEstudante(Long id, @Body Estudante estudante) {
        return estudanteService.atualizar(id, estudante);
    }

    @Delete("/{id}")
    public void excluirEstudante(Long id) {
        estudanteService.excluir(id);
    }
}
Execute a aplicação Micronaut com o comando ./mvnw mn:run
Agora você tem uma aplicação Micronaut simples para cadastrar, atualizar, buscar e excluir estudantes. Você pode testar os endpoints utilizando uma ferramenta como o Postman. Lembre-se de personalizar e expandir conforme suas necessidades e requisitos do projeto.
