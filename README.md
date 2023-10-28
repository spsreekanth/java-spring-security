# java-spring-security
@EnableWebSecurity
public class JwtSecurityConfig extends WebSecurityConfigurerAdapter {

    private final String publicKeyUrl;
    private final String audience;

    public JwtSecurityConfig(String publicKeyUrl, String audience) {
        this.publicKeyUrl = publicKeyUrl;
        this.audience = audience;
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests()
                .anyRequest()
                .authenticated()
                .and()
                .addFilterBefore(new JwtAuthenticationFilter(publicKeyUrl, audience), UsernameAndPasswordAuthenticationFilter.class)
                .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS);
    }

    @Bean
    public AuthenticationManager authenticationManager() throws Exception {
        return AuthenticationManagerBuilder.create()
                .build();
    }
}
