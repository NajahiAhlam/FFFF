<nb-card accent="primary" class="">
  <ng-container *ngIf="users">
    <nb-card-body *ngIf="errorMessage">
      <div class="alert alert-danger" role="alert">
        {{ errorMessage }}
      </div>
    </nb-card-body>
    <nb-card-header class="d-flex flex-row justify-content-between">
      <h5 class="title-animation title-heading text-uppercase my-auto p-2">Gestion des Utilisateurs</h5>
    </nb-card-header>
    <nb-card-body>
      <div class="d-flex flex-row justify-content">
        <div class="custom-context-menu">
          <button type="submit" (click)="toggleContextMenu()" class="d-flex btn custom-bg m-1" id="btn-columns">
            <div class="d-flex flex-row justify-content">
              <div class="m-1">
                Colonnes
              </div>
              <div class="m-1">
                <nb-icon pack="eva" [icon]="showColumnContextMenu?'arrow-circle-up-outlinee':'arrow-circle-down-outline'"></nb-icon>
              </div>
            </div>
          </button>
          <ul class="context-menu-box" *ngIf="showColumnContextMenu">
            <li *ngFor="let col of columns" class="column_items" (click)="showColumn(col.label)">
              <div class="d-flex flex-row justify-content">
                <div class="m-1">
                  <nb-icon pack="eva" *ngIf="displayColumns.includes(col.label)" icon="checkmark-square-2-outline"></nb-icon>
                  <nb-icon pack="eva" *ngIf="!displayColumns.includes(col.label)" icon="done-all-outline"></nb-icon>
                </div>
                <div class="m-1">
                  {{col.label}}
                </div>
              </div>
            </li>
          </ul>
        </div>
      </div>

      <ng-template [ngIf]="users">
        <table class="table w-100">
          <thead class="bg-header fw-bold">
            <tr>
              <td *ngFor="let column of columns" (click)="sortBy(column.key)" class=" p-3 border border-end-white">
                <div class="d-flex justify-content-between">
                  <div>
                    {{ column.label }}
                  </div>
                  <div [ngClass]="(keyword==column.key)? 'text-black':'text-white'">
                    <span>
                      <nb-icon *ngIf="direction" pack="eva" icon="arrow-circle-up-outline"></nb-icon>
                    </span>
                    <span>
                      <nb-icon *ngIf="!direction" pack="eva" icon="arrow-downward-outline"></nb-icon>
                    </span>
                  </div>
                </div>
              </td>
              <td colspan="2" class=" p-3 border border-end-white">Actions</td>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td *ngFor="let column of columns">
                <form [formGroup]="searchFormGroup">
                  <ng-template [ngIf]="column.key==='firstName'">
                    <input formControlName="{{column.key}}" class="form-control" placeholder="{{column.label}}" (input)="searchSubject.next()">
                  </ng-template>
                  <ng-template [ngIf]="column.key==='enabled'">
                    <nb-select formControlName="{{column.key}}" selected="" (selectedChange)="searchSubject.next()">
                      <nb-option value="true">Enabled</nb-option>
                      <nb-option value="false">Disabled</nb-option>
                    </nb-select>
                  </ng-template>
                </form>
              </td>
              <td>
                <a (click)="onAddNewUser()" class="text-decoration-none float-right">
                  <button type="button" class="d-flex btn btn-dark m-auto" (click)="onAddUser()">
                    <div class="addNewUser">Ajouter</div>
                  </button>
                </a>
              </td>
            </tr>
            <tr *ngFor="let user of users.content">
              <td *ngFor="let column of columns" class="px-4">{{ getColumnValue(user, column.key) }}</td>
              <td class="editIcon text-center">
                <nb-icon pack="eva" class="btn-details" style="color:darkorange" icon="edit-outline" (click)="onUpdateUser(user.id)"></nb-icon>
                <nb-icon pack="eva" class="btn-details" icon="trash-outline" style="color: red" (click)="onDeleteUser(user.id)"></nb-icon>
              </td>
            </tr>
          </tbody>
        </table>
        <nb-card-footer>
          <div class="d-flex justify-content-between">
            <ul class="list-group list-group-horizontal">
              <li class="list-group-item">
                <button (click)="onPageNumberChange(0)"
                        [ngClass]="users.first ? 'text-gray disabled border-light' : 'text-black'" class="btn">
                  <span><<</span>
                </button>
              </li>
              <li class="list-group-item">
                <button (click)="onPageNumberChange(users.number-1)"
                        [ngClass]="users.first ? 'text-gray disabled border-light' : 'text-black'" class="btn">
                  <nb-icon pack="eva" icon="arrow-circle-right-outline"></nb-icon>
                </button>
              </li>
              <li class="list-group-item" *ngFor='let nbr of toTotalPages(users.totalPages <9 ? users.totalPages : 9) ;let i = index'>
                <span *ngIf="(users.number <= users.totalPages-9) || (users.totalPages <9);else printlastPages">
                  <button (click)="onPageNumberChange(users.number <= 5 ? i : users.number-4+i)"
                          [ngClass]="users.number==(users.number <= 5 ? i : users.number-4 + i) ?'text-black custom-bg' : 'text-balck'"
                          class="btn ">
                    {{(users.number <= 5 ? 1 + i : users.number - 3 + i)}}
                  </button>
                </span>
                <ng-template #printlastPages>
                  <button (click)="onPageNumberChange(users.totalPages - 9 + i)"
                          [ngClass]="users.number==(users.totalPages - 9 + i) ?'text-light custom-bg' : 'text-black'"
                          class="btn ">
                    {{(users.totalPages - 8 + i)}}
                  </button>
                </ng-template>
              </li>
              <li class="list-group-item">
                <button (click)="onPageNumberChange(users.number+1)"
                        [ngClass]="users.last ? 'text-gray disabled border-light' : 'text-black'" class="btn">
                  <nb-icon icon="arrow-circle-left-outline" pack="eva"></nb-icon>
                </button>
              </li>
              <li class="list-group-item">
                <button (click)="onPageNumberChange(users.totalPages -1 )"
                        [ngClass]="users.last ? 'text-gray disabled border-light' : 'text-black'" class="btn">
                  <span>>></span>
                </button>
              </li>
            </ul>
            <div>Éléments par page :
              <nb-select size="small" placeholder="Élément par page" selected="{{data?.size}}"
                         (selectedChange)="onElementPerPageChange($event)">
                <nb-option value="10">10</nb-option>
                <nb-option value="20">20</nb-option>
                <nb-option value="50">50</nb-option>
                <nb-option value="100">100</nb-option>
              </nb-select>
            </div>
          </div>
        </nb-card-footer>
      </ng-template>
      <ng-template [ngIf]="!users">
        <nb-card-body>
          Aucun utilisateur pour le moment
        </nb-card-body>
      </ng-template>
    </nb-card-body>
    <nb-card-footer>
    </nb-card-footer>
  </ng-container>
  <ng-container *ngIf="loading">
    <!-- Loading indicator -->
  </ng-container>
</nb-card>

---------------------------------------------------------------------------------------------------------------------------------------------------

import {Injectable} from '@angular/core';
import {environment} from "../../../../environments/environment";
import {HttpClient, HttpParams} from "@angular/common/http";
import {SearchUserCriteria} from "../../../@core/models/search-user-criteria.model";
import {Observable} from "rxjs";
import {CockpitRole, User} from "../../../@core/models/user.model";
import { ObjectPagination } from 'src/app/@core/models/object.pagination.model';

@Injectable({
  providedIn: 'root'
})
export class GestionUtilisateurService {

  constructor(private _http: HttpClient) {
  }
  


  getAllUsers(pageNumber: number, elementPerPage: number, sortValue: string, sortDirection: string, searchCriteria: any)
  {
    let params = new HttpParams();
    if(searchCriteria)
    {
      Object.keys(searchCriteria).forEach((k) => {
        if (searchCriteria[k] != null && searchCriteria[k] !== '') {
          params = params.append(k, searchCriteria[k]);
        }
      });
    }
    params = params.append('pageNumber',pageNumber)
    params = params.append('pageSize',elementPerPage)
    params = params.append('sortValue',sortValue)
    params = params.append('sortDirection',sortDirection)
    return this._http.get<ObjectPagination<User>>(environment.baseUrl+"/api/cockpit/users/search",{params:params})
  }


  fetchRoles(): Observable<CockpitRole[]> {
    return this._http.get<CockpitRole[]>(environment.baseUrl+"/api/cockpit/users/roles");
  }

  getUser(id: number): Observable<User> {
    return this._http.get<User>(environment.baseUrl+"/api/cockpit/users/" + id);
  }

  createUser(user: User): Observable<User> {
    return this._http.post<User>(environment.baseUrl+"/api/cockpit/users/user", user);
  }

  updateUser(id: number, user: User) {
    return this._http.put<User>(environment.baseUrl+`/api/cockpit/users/update/${id}`,user);
  }
  toggleUserEnabled(id: number){
    return this._http.put<User>(environment.baseUrl+`/api/cockpit/users/${id}/toggle-enabled`,null);

  }
}
--------------------------------------------------------------------------------------------------------------------------
export interface ObjectPagination<T>{
    content:[T],
    totalPages:number,
    totalElements:number,
    size:number,
    number:number,
    first:boolean,
    last:boolean
  }
---------------------------------------------------------------------------------------
export interface User {
  id?: number;
  uuid?: string;
  firstName?: string;
  lastName?: string;
  matricule?: string;
  enabled?: boolean;
  email?: string;
  dateModification?: Date;
  dateStatus?: Date;
  dateCreation?: Date;
  listRoles?: string[];
  rolesIds?: number[];
  roles?:string[];
}
---------------------------------------------------------------------------------------
 public Contrat saveContrat(ContratDTO contratDTO) {
        User user = userRepository.findById(contratDTO.getUserId())
                .orElseThrow(() -> new ResourceNotFoundException("User not found with id " + contratDTO.getUserId()));
        Fournisseur fournisseur = fournisseurRepository.findById(contratDTO.getFournisseurId())
                .orElseThrow(() -> new ResourceNotFoundException("Fournisseur not found with id " + contratDTO.getFournisseurId()));
        
        Contrat contrat = new Contrat();
        contrat.setUser(user);
        contrat.setFournisseur(fournisseur);
        contrat.setDetails(contratDTO.getDetails());
        
        return contratRepository.save(contrat);
    }**

        public ContratDTO saveContrat(ContratDTO contratDTO) {
        User user = userRepository.findById(contratDTO.getUserId())
                .orElseThrow(() -> new ResourceNotFoundException("User not found with id " + contratDTO.getUserId()));
        Fournisseur fournisseur = fournisseurRepository.findById(contratDTO.getFournisseurId())
                .orElseThrow(() -> new ResourceNotFoundException("Fournisseur not found with id " + contratDTO.getFournisseurId()));
        
        Contrat contrat = contratMapper.toContrat(contratDTO);
        contrat.setUser(user);
        contrat.setFournisseur(fournisseur);
        
        Contrat savedContrat = contratRepository.save(contrat);
        
        return contratMapper.toContratDTO(savedContrat);
    }
