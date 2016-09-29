# specification-with-projection
Support Projections with `JpaSpecificationExecutor.findAll(Specification,Pageable)` for Spring Data JPA

## How to use
* add annotation `@EnableJpaRepositories(repositoryBaseClass = JpaSpecificationExecutorWithProjectionImpl.class)` on Application class (Spring Boot)
* create your repository and extends `JpaSpecificationExecutorWithProjection`
```
public interface DocumentRepository extends JpaRepository<Document,Integer>,JpaSpecificationExecutorWithProjection<Document,Integer> {
    /**
    * projection interface
    **/
    public static interface DocumentWithoutParent{
        Long getId();
        String getDescription();
        String getDocumentType();
        String getDocumentCategory();
        List<DocumentWithoutParent> getChild();
    }
}
```
* use it

  ```
      @Test
    public void specificationWithProjection() {
        Specifications<Document> where = Specifications.where(DocumentSpecs.idEq(1L));
        Page<DocumentRepository.DocumentWithoutParent> all = documentRepository.findAll(where, DocumentRepository.DocumentWithoutParent.class, new PageRequest(0,10));
        Assertions.assertThat(all).isNotEmpty();
    }
  ```
