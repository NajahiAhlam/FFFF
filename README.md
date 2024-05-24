import { Component } from '@angular/core';
import { NbWindowControlButtonsConfig, NbWindowRef, NbWindowService } from '@nebular/theme';
import { DialogContratComponent } from '../../component/dialog-contrat/dialog-contrat.component';
import { Contrat } from 'src/app/@core/models/contrat.model';
import { AppDataState, DataStateEnum } from 'src/app/@core/state';
import { Observable, catchError, defaultIfEmpty, filter, forkJoin, map, of, startWith, switchMap } from 'rxjs';
import { UtilsService } from 'src/app/@core/services/utils.service';
import { GestionContratService } from '../../services/gestion-contrat.service';
import { ActivatedRoute } from '@angular/router';
import { GestionFournisseurService } from 'src/app/pages/gestion-referentiel/services/gestion-fournisseur.service';
import { GestionUtilisateurService } from 'src/app/pages/gestion-utilisateur/service/gestion-utilisateur.service';
import { Fournisseur } from 'src/app/@core/models/fournisseur.mode';
import { Console } from 'console';

@Component({
  selector: 'app-contrat-details',
  templateUrl: './contrat-details.component.html',
  styleUrls: ['./contrat-details.component.scss']
})
export class ContratDetailsComponent  {
  requestId!:number
  data$: Observable<AppDataState<Contrat>> | undefined;
  errorMessage!: string;
  contrat!:Contrat
  readonly DataStateEnum = DataStateEnum;
  detailsCardBody:boolean=true;
  detailsFCardBody:boolean=true;
  detailsDatesCardBody:boolean=true;
  fournisseurName$!: Observable<string>;
  userName$!: String;
  userId!:number;

  constructor(private _route:ActivatedRoute,
    private windowService: NbWindowService,public gestionContratService:GestionContratService, public gestionFournisseurService: GestionFournisseurService, public gestionUserService: GestionUtilisateurService, private _utilService:UtilsService) { }

    ngOnInit(): void {
      this.requestId=this._route.snapshot.params['id']
      //this.userName$= this.getUserName(this.contrat.userId)
    //this.fournisseurName$= this.getFournisseurName(this.contrat.fournisseurId!)
   
      this.getContratDetails()
      this.getUserName(this.contrat.userId)
    }

    hideShowDetailsCardBody()
    {
      this.detailsCardBody=!this.detailsCardBody
      
    }
    hideShowDetailsFCardBody(){
      this.detailsFCardBody=!this.detailsFCardBody
    }
    hideShowDetailsDatesCardBody(){
      this.detailsDatesCardBody=!this.detailsDatesCardBody
    }
   getContratDetails()
    {
      if(this.requestId)
      {
        this.data$=this.gestionContratService.getContrat(this.requestId)
          .pipe(
            map(response => {
              this.contrat =response;
              this.userId=this.contrat.userId
              return ({dataState: DataStateEnum.LOADED, data: response})
            }),
            startWith({dataState: DataStateEnum.LOADING}),
            catchError(err => of({
              dataState: DataStateEnum.ERROR,
              errorMessage: err.message,
              this: this._utilService.displayError('Une erreur technique est survenue', "Erreur")
            }))
          );
      }
     
    } 
    /*getFournisseurName(fournisseurId: number): Observable<string> {
      return this.gestionFournisseurService.getFournisseurName(fournisseurId).pipe(
        map((response: Fournisseur) => response.nom ?? 'Unknown'), // Use nullish coalescing operator to return 'Unknown' if response.nom is null or undefined
        catchError(error => {
          console.error('Error fetching fournisseur name:', error);
          return of('Unknown');
        })
      );
    }*/

    getUserName(userId: number) {
      return this.gestionUserService.getUserName(userId).subscribe((data)=>{
        next:()=>{
          this.userName$=data
        }
        error:(er:string)=>{
          console.log(er);
        }
      });
    }

 openEditForm1() {
    console.log('id',this.requestId)
    console.log('contrat',this.data$)
    //if(!this.contrat){this.getContratDetails();}
    const buttonsConfig: NbWindowControlButtonsConfig = {
      minimize: false,
      maximize: false,
      fullScreen: true,
      close: true,
    };
    
    const nbWindowRef = this.windowService.open(DialogContratComponent, {
      title: `Modifier un contrat`,
      context: { id: this.requestId, contrat: this.contrat  }, 
      buttons: buttonsConfig,
    });

    this.initCloseListener(nbWindowRef); 
         
  
  } 
  openEditForm() {
    const buttonsConfig: NbWindowControlButtonsConfig = {
      minimize: false,
      maximize: false,
      fullScreen: false,
      close: true,
    }
    this.windowService.open(DialogContratComponent, {
      hasBackdrop: true,
      closeOnEsc: false,
      buttons: buttonsConfig,
      closeOnBackdropClick: false,
      title:'Action sur la de modification  du contrat  N°'+this.requestId,
      context:{
        request:this.contrat,
        //withFile:false
      }
    }).onClose.subscribe((data)=>{
      if(data.isEdit)
      {
        //this.upload=true
        this.gestionContratService.updateContrat(this.requestId,this.contrat).subscribe({
          next:(data)=>{
            this._utilService.displaySucess("Succée","Contrat modifié avec succès",)
            location.reload()
            //this.upload=false
          },
          error:(err=>{
            //this.upload=false
          })
        })
      }
    })
  }

  
  private initCloseListener(windowRef: NbWindowRef) {
    windowRef?.onClose.subscribe({
      next: (res) => {
        if (res === 'SUCESS') console.log('succes')
      },

      error: (err) => console.error(`Observer got an error: ${err}`),
    });
  }

 

}
