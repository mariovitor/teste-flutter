
  arquivo login_events.dart
  ```dart
 sealed  class  LoginEvent {}
class  LoginEmailChanged  extends  LoginEvent {
final  String  email;
LoginEmailChanged(this.email);
}
class  LoginPasswordChanged  extends  LoginEvent {
final  String  password;
LoginPasswordChanged(this.password);
}
class  LoginSubmitted  extends  LoginEvent {}
```
  
arquivo login_states.dart
  ```dart
class  LoginState {
	final  String  email;
	final  String  password;
	final  String  error;
	LoginState({
		this.email  =  '',
		this.password  =  '',
		this.error  =  '',
	});
	LoginState  copyWith({
		String?  email,
		String?  password,
		String?  error,
	}) {
		return  LoginState(
		email:  email  ??  this.email,
		password:  password  ??  this.password,
		error:  error  ??  this.error);
	}
}

class UserLogged extends LoginState {
  UserLogged({super.email});
}
```

arquivo login_bloc.dart
  ```dart
import 'package:bloc/bloc.dart';
import 'package:todo_list_cubit/src/presentation/login/bloc/login_events.dart';
import 'package:todo_list_cubit/src/presentation/login/bloc/login_state.dart';

class LoginBloc extends Bloc<LoginEvent, LoginState> {
  LoginBloc() : super(LoginState()) {
    on<LoginEmailChanged>((event, emit) {
      emit(state.copyWith(email: event.email, error: ''));
    });

    on<LoginPasswordChanged>((event, emit) {
      emit(state.copyWith(password: event.password, error: ''));
    });

    on<LoginSubmitted>((event, emit) {
      if (state.email.isEmpty || state.password.isEmpty) {
        emit(state.copyWith(error: 'Preencha todos os campos'));
        return;
      }

       //TODO auth
      //TODO push route
      emit(state.copyWith(error: ''));
      emit(UserLogged(email: state.email));
    });
  }
}
```

arquivo login_page.dart
  ```dart
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';
import 'package:todo_list_cubit/src/presentation/login/bloc/login_bloc.dart';
import 'package:todo_list_cubit/src/presentation/login/bloc/login_events.dart';
import 'package:todo_list_cubit/src/presentation/login/bloc/login_state.dart';

class LoginPage extends StatelessWidget {
  const LoginPage({super.key});
  static const String routeName = '/login';
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: BlocProvider(
        create: (context) => LoginBloc(),
        child: BlocConsumer<LoginBloc, LoginState>(listener: (context, state) {
          if (state is UserLogged) {
            Navigator.of(context).pushReplacementNamed('/home');
          }
        }, builder: (context, state) {
          return Padding(
            padding: const EdgeInsets.all(32.0),
            child: Form(
                child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                TextFormField(
                  decoration: const InputDecoration(
                    hintText: 'Email',
                  ),
                  onChanged: (v) {
                    context.read<LoginBloc>().add(LoginEmailChanged(v));
                  },
                ),
                Padding(
                  padding: const EdgeInsets.symmetric(vertical: 8.0),
                  child: TextFormField(
                    decoration: const InputDecoration(
                      hintText: 'Senha',
                    ),
                    onChanged: (v) {
                      context.read<LoginBloc>().add(LoginPasswordChanged(v));
                    },
                  ),
                ),
                const SizedBox(height: 24),
                if (state.error.isNotEmpty)
                  Text(
                    state.error,
                    style: const TextStyle(color: Colors.red),
                  ),
                const SizedBox(height: 24),
                ElevatedButton(
                    onPressed: () {
                      context.read<LoginBloc>().add(LoginSubmitted());
                    },
                    child: const Text('Entrar')),
              ],
            )),
          );
        }),
      ),
    );
  }
}

```
