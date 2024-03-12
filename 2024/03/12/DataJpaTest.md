# @DataJpaTest
- JPA 컴포넌트들을 테스트하기 위한 애노테이션
- ApplicationContext 전체가 아닌 JPA 에 필요한 설정들에 대해서만 Bean 으로 등록
- @Transactional 애노테이션을 포함하며 테스트 종료 시 롤백을 수행